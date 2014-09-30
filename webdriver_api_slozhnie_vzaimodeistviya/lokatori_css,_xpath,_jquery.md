# Локаторы. CSS, XPATH, JQUERY.
Ранее вы уже изучали локаторы, с помошью которых можно идентифицировать элементы на web странице. В этой главе мы пристальнее рассморим наиболее сложные из них, а именно CSS, Xpath и jQuery локаторы.

### CSS локаторы
Многие браузеры реализуют CSS движок, чтобы разработчики смогли применять CSS таблицы в своих проектах. Что позволяет разделить между собой контент сраницы с её офрмлением.
В CSS есть паттерны, согласно которым стили, создаваемые разработчиком, применяются к элементам страницы (DOM). Эти паттерны называются локаторы (selectors). Selenium WebDriver использует тот же принцип для нахождения элементов. И он намного быстрее, чем поис элементов на основе XPath, который мы рассмотрим чуть позже.

Давайте рассморим несколько бызовых локаторов, использованием CSS:

Абсолютный путь:

    WebElement userName = driver.findElement(By.cssSelector("html body div div form input"));

Относительный путь:

    WebElement userName = driver.findElement(By.cssSelector("input"));
    
Поиск по ID элемента:

    WebElement userName = driver.findElement(By.cssSelector("input#username"));
    
    или
    
    WebElement userName = driver.findElement(By.cssSelector("#username"));
    
Поиск по значениям аттрибутов html тегов:

    WebElement previousButton = driver.findElement(By.cssSelector("img[alt='Previous']"));

Посик по названиею аттрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.cssSelector("img[alt]"));


### XPath локаторы

XPath (XML path) - это язык для нодов (nodes) в XML документе. Так как многие браузеры поддерживают XHTML, то мы можем изпользовать XPath для находения элементов на web страницах.

Важным рахличием между CSS и XPath локаторами является то, что, используя XPath, мы можем производить как в глубину DOM иерархии, так и возращаться назад. Что же касается CSS, то тут мы можем двигаться только в глубину. Это значит, что с XPath можем найти родительский элемент, по дочернему.

Расмотрим некоторые примеры:

Абсолютный путь:

    WebElement userName = driver.findElement(By.xpath("html/body/div/div/form/input"));

Относительный путь:

    WebElement userName = driver.findElement(By.xpath("//input"));
    
Поиск элемента по тексту:

    WebElement userName = driver.findElement(By.xpath(".//*[text()='Первая ссылка']/.."));
    
Поиск по значениям аттрибутов:

    WebElement userName = driver.findElement(By.xpath("//input[@id='username']"));
    
Посик по названиею аттрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.xpath ("img[@alt]"));

### jQuery локаторы


Мы можем находить элементы, используя jQuery локаторы. Они используются, когда CSS локаторы
изначально не поддерживаются браузерами.

Первое, что нужно сделать для ихиспользования - проверить поддерживает ли приложение jQuery библиотеку. Если нет, то мы сами должны её добавить:

    bool flag = (bool)js.ExecuteScript("return typeof jQuery == 'undefined'");
    if (flag)
      {
          js.ExecuteScript("var jq = document.createElement('script');
          jq.src = '//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js';
          document.getElementsByTagName('head')[0].appendChild(jq);");
      }


Теперь нам нужен объект, с помощью которого мы смогли бы работать с JavaScript и, как следствие, с jQuery. Для этого мы будет использовать **JavaScriptExecutor**

    IWebDriver Driver = new InternetExplorerDriver(@"Path for IE Driver");
    Driver.Url = "http://google.com";            
    IJavaScriptExecutor js = (IJavaScriptExecutor)Driver;


Теперь всё готово, для того, чтобы мы могли использовать jQuery локаторы.

    IEnumerable<IWebElement> elements = 
        (IEnumerable<IWebElement>)js.ExecuteScript(@"return $(""input[id^='gbq']"")");

А вот и сам локатор:    

    input[id^='gbq']
    
Так же мы можем использовать CSS локаторы внутри jQuery локаторов:

    $(‘input.someclass’)
