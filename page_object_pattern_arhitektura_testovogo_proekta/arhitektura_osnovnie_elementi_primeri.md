# Архитектура. Основные элементы.

Что же такое архитектура? Это, по сути своей, структура разрабатываемой программы,  описание того, как внутри устроены компоненты и как они друг с другом взаимодействуют. Кто-то может подумать , мол это же просто автотесты, зачем тут такие сложные процессы, проектирование архитектуры. Но это очень большое заблуждение.

Отсутсвие архитекуры при проектировании тестового  фреймворка приводит к многочисленным переписываниям кода,что в свою очередь требует серьезных временых затрат. Но, как говорится, "Время - деньги". Следствие всего этого - потеря прибыли и недовольство заказчиков. Перейдём непосредственно к важным частям архитектуры.  

Центральное место в архитектуре для автоматизации занимает паттерн Page Object, с которым вы уже знакомы. Он позволяет отделить логику описания веб-страниц от логики тестов. Также не будем забывать и о Page Element, который позволяет инкапсулировать работы с кастомными элементами. 

При работе с паттерном Page Object, у нас возникает проблема: где удобнее хранить локаторы?
Решить её можно создав UI Map. Для этого можно воспользоваться вариантами описаными ниже. Один из них это использовать файлы .properties.

    .properties — текстовый формат и одноименное расширение имени файла, применяемое для сохранения конфигурационных параметров

Каждый параметр сохраняется как пара строк, первая содержит имя параметра (ключ), вторая значение параметра. Ключ и его значение в файле разделяются знаком равенства (ключ=значение). К примеру вы создали файл locators.properties, а его содержимое будет выглядеть так: 

    LoginPage.title=Login Page
    LoginPage.userNameInput=id=Login
    LoginPage.userPassInput=id=Password
    LoginPage.loginButton=css=input[value=Login]
    Grid.FieldFrom=xpath=.//input[contains(@placeholder,'From')]
    Grid.Datepicker=css=.datepicker
    HomePage.title=Games

А чтобы использовать содержимое Properties файла, мы напишем парсер, который нам в этом поможет:


    public class Locators {
        private static final Properties locators;
    
        private enum LocatorType{
            id, name, css, xpath, tag, text, partText;
        }
    
        static {
            locators = new Properties();
            InputStream is = Locators.class.getResourceAsStream("/locators.properties");
            try {
                locators.load(is);
            }
            catch (Exception e) {
                System.out.println(e.getMessage());
            }
        }
    
        public static String title(String pageName) {
            return locators.getProperty(pageName);
        }
    
        public static By get(String locatorName) {
            String propertyValue = locators.getProperty(locatorName);
            return getLocator(propertyValue);
        }
    
        public static By get(String locatorName, String parameter) {
            String propertyValue = locators.getProperty(locatorName);
            return getLocator(String.format(propertyValue, parameter));
        }
    
        private static By getLocator(String locator){
            String[] locatorItems = locator.split("=",2);
            LocatorType locatorType = LocatorType.valueOf(locatorItems[0]);
    
            switch(locatorType) {
    
                case id :{
                    return By.id(locatorItems[1]);
                }
    
                case name:{
                    return By.name(locatorItems[1]);
                }
    
                case css:{
                    return By.cssSelector(locatorItems[1]);
                }
    
                case xpath:{
                    return By.xpath(locatorItems[1]);
                }
    
                case tag:{
                    return By.tagName(locatorItems[1]);
                }
    
                case text:{
                    return By.linkText(locatorItems[1]);
                }
    
                case partText:{
                    return By.partialLinkText(locatorItems[1]);
                }
    
                default:{
                    throw new IllegalArgumentException("No such locator type: " + locatorItems[0]);
                }
            }
        }


Другой способ организации работы с локаторами - это использование Html Elements, котрый объединяет элементы в блоки. Вот как выглядит пример такого блока:

    @Name("Search form")
    @Block(@FindBy(xpath = "//form"))
    public class SearchArrow extends HtmlElement {
        @Name("Search request input")
        @FindBy(id = "searchInput")
        private TextInput requestInput;
    
        @Name("Search button")
        @FindBy(className = "b-form-button__input")
        private Button searchButton;
    
        public void search(String request) {
            requestInput.sendKeys(request);
            searchButton.click();
        }
    }


Тогда Page Object будет описывать следующим образом:

    public class SearchPage {
        private SearchArrow searchArrow;
        // Other blocks and elements here
    
        public SearchPage(WebDriver driver) {
            PageFactory.initElements(new HtmlElementDecorator(driver), this);
        }
    
        public void search(String request) {
            searchArrow.search(request);
        }
    
        // Other methods here
    }

Еще одной важной частью архитектуры является создание Helper классов (вспомогательных классов). В них мы можем пометить методы, которые мы применяем на многих страницах, чтобы избежать дублирования кода, следуя принципу Don't repeat youself. В такие классы можно поместить реализацию кастомных ожиданий или реализацию сложных действий:

    public class Waiter {
        private static final int DEFAULT_TIME_OUT = 10;
    
        public static void waitFor(final ExpectedCondition condition){
            getWaiter().until(condition);
        }
    
        public static void waitForJquery(){
            getWaiter().until(new ExpectedCondition<Boolean>() {
                @Override
                public Boolean apply(WebDriver webDriver) {
                    JavascriptExecutor js = (JavascriptExecutor) webDriver;
                    return (Boolean) js.executeScript("return jQuery.active == 0");
                }
            });
        }
        
        //other waiters
    }
    
    
Также полезным окажется применение шаблона Flow. Он заключается в том, что описывая функционал веб-страниц, в методах мы возвращаем либо саму страницу, либо объект другой страницы. Такой подход еще больше похож на то, как работает пользователь: он либо выполняет действия на странице и никуда не переходит, либо нажав на какую-либо кнопку оказывается на другой странице веб-приложения.

Вот, пожалуй, одни из основных архитектурных решений, которые упростят вам жизнь как автоматизаторам. Однако, хочется сразу сказать, что универсального решения нет. Все завситит от особеностей проекта, используемых технологий и желания заказчика. 

