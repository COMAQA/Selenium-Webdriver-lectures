# Локаторы. CSS, XPATH, JQUERY.
Ранее мы уже изучали локаторы, с помошью которых можно идентифицировать элементы на web странице. В этой главе мы пристальнее рассморим наиболее сложные из них, а именно CSS, Xpath и jQuery локаторы.

### CSS локаторы
Многие браузеры реализуют CSS движок, чтобы разработчики смогли применять CSS таблицы в своих проектах. Что позволяет разделить между собой контент сраницы с её офрмлением.
В CSS есть паттерны, согласно которым стили, создаваемые разработчиком, применяются к элементам страницы (DOM). Эти паттерны называются локаторы (selectors). Selenium WebDriver использует тот же принцип для нахождения элементов. И он намного быстрее, чем поиск элементов на основе XPath, который мы рассмотрим чуть позже.

Давайте рассморим несколько базовых локаторов с использованием CSS:

Абсолютный путь:

    WebElement userName = driver.findElement(By.cssSelector("html body div div form input"));

Относительный путь:

    WebElement userName = driver.findElement(By.cssSelector("input"));
    
Поиск по ID элемента:

    WebElement userName = driver.findElement(By.cssSelector("input#username"));
    
    или
    
    WebElement userName = driver.findElement(By.cssSelector("#username"));
    
Поиск по значениям атрибутов html тегов:

    WebElement previousButton = driver.findElement(By.cssSelector("img[alt='Previous']"));

Посик по названиею атрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.cssSelector("img[alt]"));


### XPath локаторы

XPath (XML path) - это язык для нодов (nodes) в XML документе. Так как многие браузеры поддерживают XHTML, то мы можем использовать XPath для находения элементов на web страницах.

Важным рахличием между CSS и XPath локаторами является то, что, используя XPath, мы можем производить перемещение как в глубину DOM иерархии, так и возращаться назад. Что же касается CSS, то тут мы можем двигаться только в глубину. Это значит, что с XPath можем найти родительский элемент, по дочернему.

Расмотрим некоторые примеры:

Абсолютный путь:

    WebElement userName = driver.findElement(By.xpath("html/body/div/div/form/input"));

Относительный путь:

    WebElement userName = driver.findElement(By.xpath("//input"));
    
Поиск элемента по тексту:

    WebElement userName = driver.findElement(By.xpath(".//*[text()='Первая ссылка']/.."));
    
Поиск по значениям атрибутов:

    WebElement userName = driver.findElement(By.xpath("//input[@id='username']"));
    
Посик по названию атрибутов:

    List<WebElement> imagesWithAlt = driver.findElements(By.xpath ("img[@alt]"));

### jQuery локаторы


Мы можем находить элементы, используя jQuery локаторы. Они используются, когда CSS локаторы
изначально не поддерживаются браузерами.

Первое, что нужно сделать для их использования - проверить поддерживает ли приложение jQuery библиотеку. Если нет, то мы сами должны её добавить:

    bool flag = (bool)js.ExecuteScript("return typeof jQuery == 'undefined'");
    if (flag)
      {
          js.ExecuteScript("var jq = document.createElement('script');
          jq.src = '//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js';
          document.getElementsByTagName('head')[0].appendChild(jq);");
      }


Теперь нам нужен объект, с помощью которого мы смогли бы работать с JavaScript и, как следствие, с jQuery. Для этого мы будем использовать **JavaScriptExecutor**

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
