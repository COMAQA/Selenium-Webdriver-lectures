# HttpWatch
Еще одним популярным инструментом для тестирования производительности является HttpWatch. Узнать об инструменте или скачать последнюю версию можно по адресу http://www.httpwatch.com/

Как и DynaTrace, HttpWatch собирает различные метрики клиентской производительности для анализа проблем с производительностью javascript, выполнения запросов, передачи ресурсов и загрузки страницы.

HttpWatch работает в браузерах:
* Internet Explorer 6-11
* Firefox 25-32

На платформах:
* Windows XP
* Windows Vista
* Windows 7
* Windows 8, 8.1

Заявленная поддержка языков программирования:
* C#
* Ruby
* Javascript

Для скачивания доступны платная и бесплатная версии. Различие между ними доступно по адресу http://www.httpwatch.com/editions.htm. При установке HttpWatch установит плагины для браузеров, которые нужно настроить для автоматических тестов:

На вкладке **reporting** установите радиокнопку **Start Recording when HttpWatch is opened** и отметьте чекбокс **Stop recording when HttpWatch is closed**. Изменения сохраните.

Пример кода на C# (HttpWatch.dll находится в директории установки инструмента):
```
// Создаем контроллер HttpWatch
Controller control = new Controller();
//Создаем экземпляр IE драйвера
IWebDriver driver = new InternetExplorerDriver();
// Создаем уникальный титул стартовой страницы, чтобы HttpWatch мог подключиться
string uniqueTitle = Guid.NewGuid().ToString();
IJavaScriptExecutor js = driver as IJavaScriptExecutor;
js.ExecuteScript("document.title = '" + uniqueTitle + "';");
// Подключаем HttpWatch ксозданному экземпляру IE драйвера
Plugin plugin = control.AttachByTitle(uniqueTitle);
// Открываем окно HTTPWatch
plugin.OpenWindow(false);
// Переходим по url
driver.Navigate().GoToUrl("YourUrlHere");
// Выполняем действия
driver.FindElement(...).SendKeys(...);
driver.FindElement(...).SendKeys(...);
driver.FindElement(...).Click();
// Сохраняем данные, собранные HttpWatch, в файл
plugin.Log.Save("C:\\report.hwl");
// Закрываем браузер
driver.Close();
```
Получив данные и сохранив их в файле, вы можете просмотреть их с помощью HttpWatch Studio, которая устанавливается вместе с HttpWatch. Репорт содержит данные и метрики как на скриншоте:
![](/resources/httpwatch_report.png)

Стоит также отметить тот факт, что HttpWatch репорты можно экспортировать в другие форматы, в том числе и HAR.


