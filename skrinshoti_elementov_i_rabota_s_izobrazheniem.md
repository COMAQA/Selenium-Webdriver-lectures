# Скриншоты элементов и работа с изображением
Интерфейс <code>TakesScreenshot</code> позволяет делать скриншоты целой страницы, текущего окна, видимой части страницы или всего дисплея, если это поддерживается браузером. Однако он не позволяет делать скриншоты отдельных элементов.

Мы можем расширить функциональность <code>TakesScreenshot</code> для возможности получения скриншотов элементов страницы с помощью Java Image API. Для этого можно использовать вспомогательный метод:
```
public static File captureElementBitmap(WebDriver, driver, WebElement element) throws Exception {
    // Делаем скриншот страницы 
    File screen = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
    // Создаем экземпляр BufferedImage для работы с изображением
    BufferedImage img = ImageIO.read(screen);
    // Получаем размеры элемента
    int width = element.getSize().getWidth();
    int height = element.getSize().getHeight();
    // Создаем прямоуголник (Rectangle) с размерами элемента
    Rectangle rect = new Rectangle(width, height);
    // Получаем координаты элемента
    Point p = element.getLocation();
    // Вырезаем изображенеи элемента из общего изображения
    BufferedImage dest = img.getSubimage(p.getX(), p.getY(), rect.width, rect.height);
    // Перезаписываем File screen
    ImageIO.write(dest, "png", screen);
    // Возвращаем File c изображением элемента
    return screen;
}
```
Сам тест с использованием этого метода может выглядеть так (при условии, что статический метод создан в классе Utils):
```
@Test
public void elementScreenshotTest(){
    WebDriver driver = new FirefoxDriver();
    driver.get(URL);
    WebElement element = driver.findElement(By.id("elem_id"));
    try {
        FileUtils.copyFile(Utils.captureElementBitmap(driver, element), new File("c:\\tmp\\div.png"));
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
Иногда в тестировании появляется необходимость сравнивать изображения. Например, для проверки загруженных иконок и изображений на сайте или для сравнения базового лэйаута на странице с текущим.

Webdriver обладает функционалом для снятия скриншотов, однако он не может сравнивать изображения. Для этого надо писать собственные расширения. В качестве примера рассмотрим следующий вспомогательный класс:
```
import java.awt.Image;
import java.awt.Toolkit;
import java.awt.image.PixelGrabber;

public class CompareUtil {
    public enum Result { Matched, SizeMismatch, PixelMismatch };
    static Result CompareImage(String baseFile, String actualFile) {
        Result compareResult = Result.PixelMismatch;
        Image baseImage = Toolkit.getDefaultToolkit().getImage(baseFile);
        Image actualImage = Toolkit.getDefaultToolkit().
        getImage(actualFile);
        try {
            PixelGrabber baseImageGrab = new PixelGrabber(baseImage, 0, 0, -1, -1, false);
            PixelGrabber actualImageGrab = new PixelGrabber(actualImage, 0, 0, -1, -1, false);
            int[] baseImageData = null;
            int[] actualImageData = null;
            if(baseImageGrab.grabPixels()) {
                int width = baseImageGrab.getWidth();
                int height = baseImageGrab.getHeight();
                baseImageData = new int[width * height];
                baseImageData = (int[])baseImageGrab.getPixels();
            }
            if(actualImageGrab.grabPixels()) {
                int width = actualImageGrab.getWidth();
                int height = actualImageGrab.getHeight();
                actualImageData = new int[width * height];
                actualImageData = (int[])actualImageGrab.getPixels();
            }
            System.out.println(baseImageGrab.getHeight() + "<>" +
            actualImageGrab.getHeight());
            System.out.println(baseImageGrab.getWidth() + "<>" +
            actualImageGrab.getWidth());
            if ((baseImageGrab.getHeight() != actualImageGrab.getHeight()) || (baseImageGrab.getWidth() != actualImageGrab.getWidth())){
                compareResult = Result.SizeMismatch;
            }
            else if(java.util.Arrays.equals(baseImageData,actualImageData)){
                compareResult = Result.Matched;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return compareResult;
    }
}
```
Пример теста с использованием этого класса:
```
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.*;
import org.apache.commons.io.FileUtils;
import org.junit.*;
import static org.junit.Assert.*;
import java.io.File;

public class ScreenShotTest {
    public WebDriver driver;
    private StringBuffer verificationErrors = new StringBuffer();
    
    @Before
    public void setUp() throws Exception {
        // Create a new instance of the Firefox driver
        driver = new FirefoxDriver();
    }
    
    @Test
    public void imageCompareTest() throws Exception {
        String scrFile = "FILE_PATH";
        String baseScrFile = "BASE_FILE_PATH";
        // Заходим на страницу
        driver.get(URL);
        File screenshotFile = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
        FileUtils.copyFile(screenshotFile, new File(scrFile));
        try {
            //Сравниваем изображение с базовым
            assertEquals(CompareUtil.Result.Matched, CompareUtil.CompareImage(baseScrFile, scrFile));
        } catch (Error e) {
            // Сохраняем ошибки в переменную
            verificationErrors.append(e.toString());
        }
    }
    
    @After
    public void tearDown() throws Exception {
        //Закрываем браузер
        driver.quit();
        String verificationErrorString = verificationErrors.toString();
        if (!"".equals(verificationErrorString)) {
            fail(verificationErrorString);
        }
    }
}
```


