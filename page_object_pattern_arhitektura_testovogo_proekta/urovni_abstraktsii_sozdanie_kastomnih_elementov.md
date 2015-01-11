# Уровни абстракции. Создание кастомных элементов.

## Уровни абстракции

В автоматизации часто применяются следующие абстракции:

* Page Object
* Page Element

Паттерн **Page Object** хорошо зарекомендовал себя в автоматизации. Основная идея – инкапсулировать логику поведения страницы в классе страницы. Таким образом, тесты будут работать не с низкоуровневым кодом, а с высокоуровневой абстракцией.

Плюсы Page Object:

* Разделение полномочий: вся логика страницы описывается в Page Object классах, а тестовые классы лишь используют их публичные методы и проверяют результат.
* DRY – все локаторы помещаются в одном месте 
* Инкапсуляция работы с драйвером. Полезно при кросс-браузерном тестировании
* Page Objects позволяет записать локаторы в декларативном стиле

В классическом варианте, паттерн предполагает создание одного класса на одну страницу. Это может быть неудобно в ряде случаев:

* Использование кастомных элементов при создании веб-приложений
* Присутсвие кастомных элементов на многих страницах

В решении этой проблемы может помочь использование наследования, но агрегация видится предпочтительнее. Поэтому лучше воспользоваться паттерном – Page Element. Page Elements – позволяет дробить страницу на более мелкие составляющие – блоки, виджеты и т.д. После чего эти блоки можно переиспользовать в нескольких страницах.


## Кастомные элементы

WebDriver очень мощный инструмент, который позволяет выполнять различные действия над элементами страниц. Для этого используется класс WebElement. Но, как и всегда, не все так просто и замечательно. На практике автоматизаторы  сталвкиваются с тем, что разработчки используют кастомные элементы, с которыми класс WebElement не работает. К примеру, возьмем написание тестов для таблиц. Стандартные классы WebDriver не работают с таблицами, потому нам нужно написать свой. В котором мы опишем работу с элеметами таблицы (ячейки, строки и т. д.). 


    import java.util.List;
    
    public class WebTable {
        private WebElement _webTable;
        
        public WebTable(WebElement webTable)
        {
            set_webTable(webTable);
        }
        
        public WebElement get_webTable() {
            return _webTable;
        }
        
        public void set_webTable(WebElement _webTable) {
            this._webTable = _webTable;
        }
        
        public int getRowCount() {
            List<WebElement> tableRows = _webTable.findElements(By.tagName("tr"));
            return tableRows.size();
        }
        
        public int getColumnCount() {
            List<WebElement> tableRows = _webTable.findElements(By.tagName("tr"));
            WebElement headerRow = tableRows.get(0);
            List<WebElement> tableCols = headerRow.findElements(By.tagName("td"));
            return tableCols.size();
        }
        
        public WebElement getCellEditor(int rowIdx, int colIdx, int editorIdx) {
            try {
                List<WebElement> tableRows = _webTable.findElements(By.tagName("tr"));
                WebElement currentRow = tableRows.get(rowIdx-1);
                List<WebElement> tableCols = currentRow.findElements(By.tagName("td"));
                WebElement cell = tableCols.get(colIdx-1);
                WebElement cellEditor = cell.findElements(By.tagName("input")).get(editorIdx);
                return cellEditor;
            } catch (NoSuchElementException e) {
                throw new NoSuchElementException("Failed to get cell editor");
            }
        }
    }

