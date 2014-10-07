# Geb
Geb - это инструмент для автоматизации браузера, написанный на скриптовом языке Groovy (JVM-based) и использующий Selenium Webdriver для автоматизации браузера, JQuery селекторы для локации элементов и page object паттерн. В рамках автотестирования он легко интегрируется с различными тестовыми фреймворками, как JUnit, TestNG, Spock.

В сравнении с Webdriver API, geb предоставляет более удобный интерфейс в следующих областях:
* работа с экземпляром Webdriver (создание, настройка, переходы, уничтожение)
* нахождение элементов (JQuery локаторы)
* page object паттерн
* ожидания
* взаимодействия со страницей
* работа с AJAX элементами
* интеграция с build инструментами (maven, gradle, grails)
* интеграция с облачными сервисами (Sauce labs, Browser Stack)

Пример теста на Geb:
```
import geb.Browser
 
Browser.drive {
    go "http://myapp.com/login"
     
    assert $("h1").text() == "Please Login"
     
    $("form.login").with {
        username = "admin"
        password = "password"
        login().click()
    }
     
    assert $("h1").text() == "Admin Section"
}
```
Geb разрабатывался для удобной и быстрой автоматизации. Полее подробно об этом инструменте можно найти на официальном сайте http://www.gebish.org/