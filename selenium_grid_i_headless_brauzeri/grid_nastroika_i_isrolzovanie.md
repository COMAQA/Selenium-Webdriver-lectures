# Grid. Настройка и использование.

###Описание

**Selenium Grid** – это кластер, включающий в себя несколько Selenium-серверов. Он позволяет организовать распределённую сеть, позволяющую параллельно запускать много браузеров на разных машинах.

Selenium Grid работает следующим образом: имеется центральный сервер (hub), к которому подключены узлы (node). Работа с кластером осуществляется через hub, при этом он просто транслирует запросы узлам. Узлы могут быть запущены на той же машине, что и hub или на других.

Сервер и узлы могут работать под управлением разных операционных систем, на них могут быть установлены разные браузеры. Одна из задач Selenium Grid заключается в том, чтобы «подбирать» подходящий узел по типу браузера, версии, операционной системы и дургим атрибутам, заданным при старте браузера.

###Начало работы с Selenium Grid

Сначала мы должны запустить центральный сервер (hub). Это делается с помощью следующей команды:

    java -jar selenium-server-standalone-2.11.0.jar -role hub

После запуска команды можно перейти на панель администрирования хаба по адресу:

    http://localhost:4444/grid/

Запускаем узлы кластера:

    java -jar selenium-server-standalone-2.11.0.jar -role webdriver -hub http://localhost:4444/grid/register -port 5556

Если хаб запущен на другой машине, нужно заменить адрес в параметр hub. Если запускается несколько узлов на одной машине, указывайте разные значения параметра port.

Центральный сервер (hub) можно настраивать через JSON файлы:

    {
      "host": null,
      "port": 4444,
      "newSessionWaitTimeout": -1,
      "servlets" : [],
      "prioritizer": null,
      "capabilityMatcher": "org.openqa.grid.internal.utils.DefaultCapabilityMatcher",
      "throwOnCapabilityNotPresent": true,
      "nodePolling": 5000,

      "cleanUpCycle": 5000,
      "timeout": 300000,
      "maxSession": 25,
      "maxInstances": 25
    }

Настройка узлов (nodes) дополнительно включает в себя информацию о поддерживаемых браузерах:

    {
      "capabilities":
          [
            {
              "browserName": "firefox",
              "maxInstances": 5,
              "seleniumProtocol": "WebDriver"
            },

            {
              "browserName": "chrome",
              "maxInstances": 5,
              "seleniumProtocol": "WebDriver"
            }
          ],
      "configuration":
      {
        "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
        "maxSession": 25,
        "port": 5555,
        "host": hostIp,
        "register": true,
        "registerCycle": 5000,
        "hubPort": 4444
        "hubHost" : hubIp
      }
    }
    
На этом настройка самого грида заканчивается. Следующая стадия - настройка автотестов на использование Selenium Grid, вместо локальных браузеров. Для того, чтобы указать, на какой платформе и в каком браузере необходимо будет запустить тесты, используем DesiredCapabilities:
   
     DesiredCapabilities capabilities = new DesiredCapabilities();
     capabilities.setPlatform(Platform.VISTA);//Grid воспринимает Windows-7 как Vista
     capabilities.setBrowserName("firefox");
     capabilities.setVersion("43");
Эти настройки теперь нам необходимо передать создаваемому экземпляру драйвера. При этом мы должны создать экземпляр не привычного нам Firefox/Chrome/InternetExplorerDriver, а использовать класс RemoteWebDriver:
 
     WebDriver driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), capabilities);
     
 Как видно, здесь же мы передаем путь к самому хабу, который будет распределять наши тесты по нодам. Здесь необходимо заменить localhost на ip-адрес машины, на которой развернут хаб и, если вы использовали не дефолтный (4444) порт, придется заменить и его на правильный. Остальная часть ссылки обязательна.
 
 Это все изменения в тестах, которые необходимо будет сделать, для запуска с использованием Selenium Grid. Как видите, при правильном подходе к архитектуре, потребуется внести изменения только в конфигурационный @Before...-метод. Возможно еще написать небольшой вспомогательный метод для получения и передачи конфигураций через консоль.
 
 Подробную информация по настройке Selenium Grid вы найдете по следующим ссылкам:
 http://www.seleniumhq.org/docs/07_selenium_grid.jsp
 
 https://github.com/SeleniumHQ/selenium/wiki/Grid2
 
