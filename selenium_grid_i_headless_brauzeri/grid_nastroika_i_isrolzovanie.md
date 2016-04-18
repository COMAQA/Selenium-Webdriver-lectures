# Grid. Настройка и использование.

###Описание

**Selenium Grid** – это кластер, включающий в себя несколько Selenium-серверов. Он позволяет организовать распределённую сеть, позволяющую параллельно запускать много браузеров на разных машинах.

Selenium Grid работает следующим образом: имеется центральный сервер (hub), к которому подключены узлы (node). Работа с кластером осуществляется через hub, при этом он просто транслирует запросы узлам. Узлы могут быть запущены на той же машине, что и hub или на других.

Сервер и узлы могут работать под управлением разных операционных систем, на них могут быть установлены разные браузеры. Одна из задач Selenium Grid заключается в том, чтобы «подбирать» подходящий узел по типу браузера, версии, операционной системы и дургим атрибутам, заданным при старте браузера.

###Начало работы с Selenium Grid

Сначала мы должны запустить центарльный сервер (hub). Это делается с помощью следующей команды:

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
