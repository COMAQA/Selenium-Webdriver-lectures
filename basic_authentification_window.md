# Basic Authentification Window
Basic авторизация  - это самый простой способ ограничения доступа к веб-приложениям и документам, предусмотренный стандартом протокола HTTP. При попытке обращения к таким ресурсам браузер формирует диалоговое окно, в котором предлагается ввести свой логин и пароль, после чего запрос выполняется повторно с предоставлением серверу данных для идентификации.

![](/resources/basic_auth_prompt.png)

Cайты, использующие basic авторизацию, встречаются нечасто, чаще это веб-приложения, используемые для административных или корпоративных целей.

Основная проблема при тестировании таких приложений - это нативные диалоговые окна, при появлении которых, также как и алертов, дальнейшее управление браузером с помощью Selenium становится просто невозможным.

Одним из способов избежать появления диалогового окна может быть
использование специального URL формата http://username:password@example.com/ для передачи учетных данных. Эта конструкция все еще поддерживается некоторыми браузерами, однако такое решение является официально устаревшим и иногда может отрицательно влиять на корректное выполнение дальнейших запросов на странице.

###Browser Mob Proxy###
При использовании basic аутентификации имя пользователя и пароль включаются в состав веб-запроса в стандартный HTTP заголовок "Authorization". Поэтому для успешного прохождения авторизации достаточно изменить HTTP заголовок перед отправкой на сервер. Сам Selenium не умеет манипулировать отправляемыми запросами, но для этих целей отлично подойдет прокси-сервер, в частности BrowserMob Proxy, в силу простоты его подключения.  В случае basic авторизации net.lightbody.bmp.proxy.ProxyServer предоставляет метод для ее автоматического выполнения:
```
server.autoBasicAuthorization("example.com", "username", "password");
```
Первый аргумент autoBasicAuthorization это не URL, а доменное имя. Он не должен содержать http:// или других частей URL. Для того, чтобы использовать учетные данные для любого домена нужно оставить первый параметр пустым.

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
        server.autoBasicAuthorization("example.com", "username", "password");
        server.start();

        // получение Selenium proxy
        Proxy proxy = server.seleniumProxy();

        // конфигурация FirefoxDriver для использования прокси
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(CapabilityType.PROXY, proxy);

        WebDriver driver = new FirefoxDriver(capabilities);

        // открытие страницы
        driver.get("http://example.com/index");

        // здесь основная часть теста

        driver.quit();
        server.stop();
    }
}
```
Главное преимущество Browser Mob Proxy при работе с basic-аутентификацией - простота и кроссплатформенность (работает на Windows и Unix-based системах).
Однако Browser Mob Proxy не предоставляет методов для работы с другими протоколами (Kerberos, NTLM).
###AutoIT###
Для автоматизации прохождения basic-авторизации можно также использовать AutoIt: нужно перехватить диалоговое окно, ввести учетные данные и подтвердить ввод. Казалось бы ничего сложного, но каждый браузер для авторизации формирует окна разных классов: Firefox использует свои диалоговые окна, IE — класс диалоговых окон Windows, а Chrome и вовсе не создает отдельного окна для ввода. В этом можно убедиться используя утилиту AutoIt Window Info. В единичном случае, например, для Firefox скрипт AutoIt может выглядеть следующим образом:
```
Local $classForBasicAuthWindow = "[CLASS:MozillaDialogClass]";

;ожидание появления окна 10 секунд
WinWait($classForBasicAuthWindow, "", 10)

If WinExists($classForBasicAuthWindow) Then
    WinActivate($classForBasicAuthWindow)

    ;имя пользователя
    Send($CmdLine[1] & "{TAB}");

    ;пароль
    Send($CmdLine[2] & "{ENTER}");

EndIf
```
В случае с авторизацией процесс AutoIt нужно запускать до открытия страницы драйвером, чтобы появившееся окно не заблокировало выполнение теста (скрипт скомпилирован в auth.exe):
```
public class SimpleBawTest {

    private final String username = "username";
    private final String password = "password";

    @Test
    public void bawTest() throws Exception {
        WebDriver driver = new FirefoxDriver();
        File autoIt = new File("/auth.exe");

        // запуск exe с передачей учетных данных в качестве параметра
        Process p = Runtime.getRuntime().exec(autoIt.getAbsolutePath()
        + " " + username + " " + password);

        driver.get("https://example.com");
        driver.findElement(By.className("sign-out"));
        driver.quit();
    }
}
```
AutoIt работает с графическим окном (UI), а не через протокол, поэтому с его помощью можно автоматизировать различные нативные окна, но его главный недостаток - он работает только на Windows платформе, что автоматически лишает тесты кроссплатформенности.

