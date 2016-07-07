# Navigation timing API
Navigation timing - javascript API для измерения производительности веб приложений, утвержденный организацией W3C в качестве стандарта.

Navigation timing предоставляет простой и прямой способ получения точных данных о загрузке страницы (page navigation) и событиях при загрузке страницы (load events). Этот API доступен в IE 9, firefox, chrome и webkit-based браузерах.

Доступ к API можно получить через свойства интерфейса <code>window.performance.timing</code> с помощью javascript. Каждый атрибут объекта <code>performance.timing</code> хранит время того или иного навигационного события, когда был послан запрос на сервер (request), в милисекундах в формате UTC (в миллисекундах с первого января 1970 года). Ноль означает, что событие не произошло.

Очередность событий при загрузке страницы изображена на диаграмме:
![](/resources/NavigationAPI.png)

Более подробно про эти события можно прочитать в самом стандарте Navigation Timing: http://www.w3.org/TR/navigation-timing/

Пример:
```
@Test
public void testLogin() {
    Webdriver driver = new FirefoxDriver();
    driver.get(SOME_URL);
    JavascriptExecutor js = (JavascriptExecutor) driver;
    // Получаем время Load Event End (окончание загрузки страниы)
    long loadEventEnd = (Long) js.executeScript("return window.performance.timing.loadEventEnd;");
    // Получаем Navigation Event Start (начало перехода)
    long navigationStart = (Long) js.executeScript("return window.performance.timing.navigationStart;");
    // Разница между Load Event End и Navigation Event Start - это время загрузки страницы
    System.out.println("Page Load Time is " + (loadEventEnd - navigationStart)/1000 + " seconds.");
}
```

