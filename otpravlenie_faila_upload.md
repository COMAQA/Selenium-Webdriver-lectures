# Отправление файла (upload)
Отправление (upload) файла обычно является довольно простой задачей.
Она сводится к нахождению <code>input</code> элемента с атрибутом <code>type = "file"</code>. Далее нужно ввести путь к файлу и нажать кнопку "submit".

Например:
```
import org.openqa.selenium.*;
import java.net.URL;

public class SimpleFileUploadTest {
    @Test
    public void uploadTest() throws Exception {
        WebDriver driver = new FirefoxDriver(capabilities);
        driver.get("http://the-internet.herokuapp.com/upload");
        WebElement upload = driver.findElement(By.id("file-upload"));
        upload.sendKeys("your/path/here.file");
        driver.findElement(By.id("file-submit")).click();
        //make assertions here
        driver.quit();
    }
}
```
Однако есть одна особенность. Если тесты будут запускаться удаленно, то необходима дополнительная настройка при создании Webdriver:
```
DesiredCapabilities capabillities = DesiredCapabilities.firefox();
driver = new RemoteWebDriver(new URL(REMOTE_HUB_URL),capabillities);
driver.setFileDetector(new LocalFileDetector());
```
Метод <code>setFileDetector</code> говорит вебдрайверу, что файл загружается с локальной машины на удаленный сервер вместо обычного указания локального пути к файлу. В таком случае, вебдрайвер отправит файл, закодированный в base64 формате, по JSON Wire протоколу на сервер прежде, чем вводить путь к файлу.
