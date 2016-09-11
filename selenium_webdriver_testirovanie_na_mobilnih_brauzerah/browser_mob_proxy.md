# Browser Mob Proxy
Ранее мы уже упоминали этот инструмент, но сейчас нас интересует его возможность собирать данные о производительности. Browser Mob Proxy может собирать данные о производительности и сохранять их в формате HAR.

HAR - это архив, содержащий данные о навигации по world wide web (или AUT), в том числе и данные о клиентской производительности, в формате JSON определенной структуры.

Сохраняются данные следующим образом:
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

        // создание HAR с меткой
        server.newHar("mypage.ru");

        // открытие страницы
        driver.get("http://example.com/index");

        // получение данных HAR
        Har har = server.getHar();

        // здесь будет обработка полученных данных
        // например, сохранение файл
        proxy.getHar().writeTo(new File("D://Test.har"));

        driver.quit();
        server.stop();
    }
}
```
Полученный HAR файл содержит JSON данные, которые можно использовать как текстовую информацию, либо просмотреть в удобном представлении, используя один из существующих инструментов для визуализации (HAR viewer). Например, по адресу http://www.softwareishard.com/har/viewer/

Получить информацию можно и без сохранения HAR файла. Java класс <code>net.lightbody.bmp.core.har.HarLog</code> предоставляет набор методов для выборочного получения нужной информации:
```
Har har = proxy.getHar();

// получить информацию о браузере
System.out.println(har.getLog().getBrowser().getName());
System.out.println(har.getLog().getBrowser().getVersion());

// список всех обработанных запросов
for (HarEntry entry : har.getLog().getEntries()) {

    System.out.println(entry.getRequest().getUrl());
    // время ожидания ответа от сервера в миллисекундах
    System.out.println(entry.getTimings().getWait());
    // время чтения ответа от сервера в миллисекундах
    System.out.println(entry.getTimings().getReceive());
}
```

