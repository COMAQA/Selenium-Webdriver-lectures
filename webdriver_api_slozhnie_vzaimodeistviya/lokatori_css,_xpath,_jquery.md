# Локаторы. CSS, XPATH, JQUERY.
Ранее мы уже изучали локаторы, с помощью которых можно идентифицировать элементы на web странице. В этой главе мы пристальнее рассморим наиболее сложные из них, а именно CSS, Xpath и jQuery локаторы.

### CSS локаторы
Многие браузеры реализуют CSS движок, чтобы разработчики смогли применять CSS таблицы в своих проектах. Что позволяет разделить между собой контент страницы с её оформлением.
В CSS есть паттерны, согласно которым стили, создаваемые разработчиком, применяются к элементам страницы (DOM). Эти паттерны называются локаторы (selectors). Selenium WebDriver использует тот же принцип для нахождения элементов. И он намного быстрее, чем поиск элементов на основе XPath, который мы рассмотрим чуть позже.

Давайте рассморим несколько базовых локаторов с использованием CSS:

Абсолютный путь:

    WebElement userName = driver.findElement(By.cssSelector("html body div div form input"));

Относительный путь:

    WebElement userName = driver.findElement(By.cssSelector("input"));
    
Поиск непосредственного дочернего элемента:
    
    WebElement userName = driver.findElement(By.cssSelector("div>a"));
    
Поиск дочернего элемента любого уровня:

    WebElement userName = driver.findElement(By.cssSelector("div a"));
    
Поиск по ID элемента:

    WebElement userName = driver.findElement(By.cssSelector("input#username"));
    
    //или
    
    WebElement userName = driver.findElement(By.cssSelector("#username"));
    
Поиск по классу:

    WebElement userName = driver.findElement(By.cssSelector("input.classname"));
    
    //или
    
    WebElement userName = driver.findElement(By.cssSelector(".classname"));

Поиск по значениям атрибутов html тегов:

    WebElement previousButton = driver.findElement(By.cssSelector("img[alt='Previous']"));

Поиск по названию атрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.cssSelector("img[alt]"));
    
Поиск по началу строки:

    WebElement previousButton = driver.findElement(By.cssSelector("header[id^='page-']"));
    
Поиск по окончанию строки:

    WebElement previousButton = driver.findElement(By.cssSelector("header[id$='page-']"));

Поиск по частичному совпадению строки:

    WebElement previousButton = driver.findElement(By.cssSelector("header[id*='page-']"));


### XPath локаторы

XPath (XML path) - это язык для нодов (nodes) в XML документе. Так как многие браузеры поддерживают XHTML, то мы можем использовать XPath для нахождения элементов на web страницах.

Важным различием между CSS и XPath локаторами является то, что, используя XPath, мы можем производить перемещение как в глубину DOM иерархии, так и возвращаться назад. Что же касается CSS, то тут мы можем двигаться только в глубину. Это значит, что с XPath можем найти родительский элемент, по дочернему.

Рассмотрим некоторые примеры:

Абсолютный путь:

    WebElement userName = driver.findElement(By.xpath("html/body/div/div/form/input"));

Относительный путь:

    WebElement userName = driver.findElement(By.xpath("//input"));
    
Поиск непосредственного дочернего элемента:
    
    WebElement userName = driver.findElement(By.xpath("//div/a"));
    
Поиск дочернего элемента любого уровня:

    WebElement userName = driver.findElement(By.xpath("//div//a"));
    
Поиск элемента по тексту:

    WebElement userName = driver.findElement(By.xpath(".//*[text()='Первая ссылка']/.."));
    
Поиск по значениям атрибутов:

    WebElement userName = driver.findElement(By.xpath("//input[@id='username']"));
    
Поиск по названию атрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.xpath ("img[@alt]"));
    
Поиск родительского элемента:

    WebElement userName = driver.findElement(By.xpath("//input[@id='username']/.."));

### jQuery локаторы


Мы можем находить элементы, используя jQuery локаторы. Они используются, когда CSS локаторы
изначально не поддерживаются браузерами.

Первое, что нужно сделать для их использования - проверить, поддерживает ли приложение jQuery библиотеку. Если нет, то мы сами должны её добавить:

    private static void addJQuery (JavascriptExecutor js) {

        String script = "";

        boolean needInjection = (Boolean)(js.executeScript("return this.$ === undefined;"));
        if(needInjection) {
            URL u = Resources.getResource("jquery.js");
            try {
                script = Resources.toString(u, Charsets.UTF_8);
            } catch(IOException e) {
                e.printStackTrace();
            }
            js.executeScript(script);
        }
    }


Теперь нам нужен объект, с помощью которого мы смогли бы работать с JavaScript и, как следствие, с jQuery. Для этого мы будем использовать **JavaScriptExecutor**

    WebDriver driver = new FirefoxDriver();
    driver.manage().window().maximize();

    JavascriptExecutor js = (JavascriptExecutor)driver;


Теперь всё готово, для того, чтобы мы могли использовать jQuery локаторы.

    WebElement element = (WebElement) js.executeScript("return jQuery.find('#hplogo');");

А вот и сам локатор:    

    '#hplogo'
    
Так же мы можем использовать CSS локаторы внутри jQuery локаторов:

    jQuery.find('input.someclass')
