# Загрузка файла
Иногда в автоматизации тестирования приходится проверять загрузку файлов (download). Главное правило, касающееся этого функционала:
<strong>избегайте загрузки файлов в ваших тестах</strong>.

Дело в том, что как правило, протестировать то, что вы хотите,можно без непосредственого сохранения на диск файлов.

Пример:
```
package com.lazerycode.selenium.Tests;
import com.lazerycode.selenium.PageObjects.LoginPage;
import com.lazerycode.selenium.PageObjects.SecretFiles;
import com.lazerycode.selenium.SeleniumBase;
import com.lazerycode.selenium.Utility.FileDownloader;
import org.testng.annotations.Test;
import java.io.File;
import static com.lazerycode.selenium.Utility.CheckFileHash.getFileHash;
import static com.lazerycode.selenium.Utility.TypeOfHash.SHA1;
import static com.thoughtworks.selenium.SeleneseTestNgHelper.assertEquals;

public class LogInGetAFileAndCheckItTest extends SeleniumBase {

    @Test
    public void logInAndDownloadSecretFile() throws Exception {
        getDriverObject().get("http://localhost:8080/downloads/");
        LoginPage loginPage = new LoginPage();
        SecretFiles secretFilesPage = loginPage.logIn("foo", "bar");
        FileDownloader fileDownloader = new FileDownloader(getDriverObject());
        fileDownloader.setURI(secretFilesPage.getSecretFileHREF());
        File secretFile = fileDownloader.downloadFile();
        int httpStatusCode = fileDownloader.getLastDownloadHTTPStatus();
        assertEquals(httpStatusCode, 200);
        assertEquals(getFileHash(secretFile, SHA1), ("781811ab9052fc61e109012acf5f22da89f2a5be"));
    }
}
```

Пример взят отсюда https://github.com/Ardesco/What-Did-You-Download. В нем используется кастомное расширение для Webdriver <code>FileDownloader</code>, который позволяет загрузить файл как временный по атрибуту <code>href="путь-к-файлу"</code>, убедиться, что загрузка была успешна (<code>assertEquals(httpStatusCode, 200);</code>), а также сравнить содержимое файла с эталонным, сравнив их хеши (<code>assertEquals(getFileHash(secretFile, SHA1),("781811ab9052fc61e109012acf5f22da89f2a5be"));</code>). Исходный код расширения можно найти по адресу https://github.com/Ardesco/Powder-Monkey

