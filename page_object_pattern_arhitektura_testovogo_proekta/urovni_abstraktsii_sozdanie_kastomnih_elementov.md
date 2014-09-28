# Уровни абстракции. Создание кастомных элементов.

Разрабатывая ПО или тестовый фреймворк мы должны помнить, что описывая разные сущности мы обязаны вводить абстракции, чтобы было потом легче заниматься поддержкой написанного кода. 

Уровень абстракции — это инкапсуляция в метод или обект реализации действия или сущности, используемых в программе. Чтобы в дальнейшем при изменение реализации действия не пришлось меня код в огромном количестве мест. Давайте посмотрим насколько это полезно в автоматизации.

WebDriver очень моный инструмент, который позволяет выполнять различные действия над элементами страниц. Для этого используется класс WebElement. Но, как и всегда, не все так просто и замечательно. На практике автоматизаторы  сталвкиваются с тем, что разработчки используют кастомные элементы, с которыми класс WebElement не работает. Тут нам и пригодится умение описывать элементы с помощью абстракций. К примеру, возьмем написание тестов для таблиц. Стандартные классы WebDriver не работают с таблицами, потому нам нужно написать свой. В котором мы опишем работу с элеметами таблицы (ячейки, строки и т. д.). 

    public class Grid {
        private static final String column = "//div[contains(@class,'ngHeaderCell') and .//td[(text()='%s')]]";
        private static final By searchField = get("Grid.SearchField");
        private static final By dropDownList = get("Grid.DropDownList");
        private static final By arrowDown = get("Games.SortButtonDown");
        private static final By arrowUp = get("Games.SortButtonUp");
        private static final By rows  = get("Games.GridContainerRows");
        private static final String cellsOfConcreteColumn = ".ngRow .ngCell.%s";
        private static final By fieldTo = get("Grid.FieldTo");
        private static final By fieldFrom = get("Grid.FieldFrom");
        private static final By datepicker = get("Grid.Datepicker");

        public static SelenideElement getColumn(String name){
            return $(By.xpath(String.format(column, name)));
        }
    
        public static SelenideElement getSearchField(String columnName){
            return getColumn(columnName).$(searchField);
        }
    
        public static SelenideElement getDropDownList(String columnName){
            return getColumn(columnName).$(dropDownList);
        }
    
        public static SelenideElement getFieldTo(String columnName){
            return getColumn(columnName).$(fieldTo);
        }
    
        public static SelenideElement getFieldFrom(String columnName){
            return getColumn(columnName).$(fieldFrom);
        }
    
        public static SelenideElement getDatepicker(){
            return $(datepicker);
        }
    
        public static ElementsCollection getCells(String colunmName){
            return $$(String.format(cellsOfConcreteColumn, getUniqueCssClass(colunmName)));
        }

        public static ElementsCollection getRows(){
            return $$(rows);
        }
    
        public static void SortColumnInDirectOrder(String name){
            getWebDriver().navigate().refresh();
            getColumn(name).$(arrowUp).click();
            Waiter.waitForJquery();
        }
    
        public static void SortColumnInInverseOrder(String name){
            getWebDriver().navigate().refresh();
            getColumn(name).$(arrowDown).click();
            Waiter.waitForJquery();
        }
    
        public static void verifyColumnPlace(String columnName, String id){
            Assert.assertEquals(getUniqueCssClass(columnName), id);
        }
    
        public static void RefreshBrowser(){
            getWebDriver().navigate().refresh();
            Waiter.waitForJquery();
        }
    
        private static String getUniqueCssClass(String columnName){
            String classValue = getColumn(columnName).getAttribute("class");
            String[] classes = classValue.split(" ");
            String result = "";
            for(String str : classes){
                if (str.contains("col")){
                    result = str;
                    break;
                }
            }
            return result;
        }
    }

