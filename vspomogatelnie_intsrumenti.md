# Вспомогательные инструменты
###AutoIt###

AutoIt - это бесплатный простой и легковесный инструмент для автоматизации графических windows приложений. Он построен на похожем на BASIC скриптовом языке, с помощью которого симулируются нажатия клавиш, движение мыши и манипуляции с окнами и контролами для автоматизации тех или иных задач.

В комплект также входит:
* AutoIt Window Info - инструмент для получения информации об окне и контролах, их атрибутах, необходимых для взаимодействия с ними.
* SciTE4AutoIt3 - инструмент для содания и редактирования скриптов.
* Aut2Exe - инструмент для компилирования AutoIt скриптов в запускаемые .exe файлы.

Скачать последнюю версию инструмента, а также найти детальную инвормацию о нем можно на официальном сайте: https://www.autoitscript.com/site/

AutoIt скрипт для работы с окном загрузки файла может выглядеть следубщим образом:
```
WinWaitActive("Open", "", "20")
If WinExists("Open") Then
    ControlSetText("Open", "", "Edit1", $CmdLine[1])
    ControlClick("Open", "", "&Open")
EndIf
```
Скомпилировав скрипт в FileDownLoadHandler.exe, его можно вызвать в тесте после появления диалога загрузки файла следующим образом:
```
Runtime.getRuntime().exec(new String[] {"FileDownLoadHandler.exe", "\"C:\\Picture.png\""})
```
###Browser Mob Proxy###
Browser Mob Proxy - это бесплантный прокси-сервер для веб браузера, с помощью которого можно отслеживать трафик, перехватывать и модифицировать запросы, создавать черные и белые списки ресурсов, имитировать медленную скорость соединения, собирать данные о производительности. Browser Mob Proxy можно использовать вместе с Selenium Webdriver или независимо.

Управлять прокси-сервером можно напрямую через Java интерфейс или через REST API. В мавен проект добавить зависимость можно указав в pom.xml файле:
```
<dependency>
    <groupId>net.lightbody.bmp</groupId>
    <artifactId>browsermob-proxy</artifactId>
    <version>2.0-beta-8</version>
    <scope>test</scope>
</dependency>
```
или
```
<dependency>
    <groupId>net.lightbody.bmp</groupId>
    <artifactId>browsermob-proxy</artifactId>
    <version>2.0-beta-8</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
если вы используете свою версию Selenium Webdriver.

Сам тест же может выглядеть так:
```
import net.lightbody.bmp.core.har.Har;
import net.lightbody.bmp.proxy.ProxyServer;

import org.junit.Test;
import org.openqa.selenium.Proxy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.remote.CapabilityType;
import org.openqa.selenium.remote.DesiredCapabilities;

public class SimpleTest {

    @Test
    public void bmpTest() throws Exception {
        // запуск прокси сервера
        ProxyServer server = new ProxyServer(4444);
        server.start();

        // получение Selenium proxy
        Proxy proxy = server.seleniumProxy();

        // конфигурация FirefoxDriver для использования прокси
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(CapabilityType.PROXY, proxy);

        WebDriver driver = new FirefoxDriver(capabilities);

        // открытие страницы
        driver.get(SOME_URL);

        // здесь основная часть теста

        driver.quit();
        server.stop();
    }
}
```
Официальный сайт Browser Mob Proxy http://bmp.lightbody.net/

