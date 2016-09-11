# DDT подход

Рассмотрим классическую ситуацию - тестирование страницы логина на сайт. Нам нужно заполнить два поля username и password, а затем нажать кнопку sign in. Данный сценарий представляется очень простым и написание для него теста не должно занять много времени. Но все мы прекрасно знаем, что данные подаваемые на вход могут быть различные: это и неверные username и password, и запрещенные символы, и просто поля могут оставить пустыми. Что же делать автоматизатору в таком случае? Писать отдельные тесты для каждого возможного ввода? А если вариантов будет 200 или 300? Это чистой воды утопия. Чтобы избежать подобных проблем существует следующий подход к тестированию, который называется Data Driven Testing (DDT). DDT позволяет данные хранить отдельно от тестов. Наш написанный тест каждый раз читает данные из хранилища (базаданных и т.д.) и выполняется, используя их. Продолжается это до тех пор, пока тесты не будут запущены со всеми данными.

WebDriver предназначен для работы только лишь с API браузеров. Поэтому, чтобы использовать в тестировании подход DDT, нам понадобятся сторонние фреймворки. Рассмотрим наиболее популярные при написании JUnit и TestNG.

## JUnit и DDT

JUnit — библиотека для модульного тестирования программного обеспечения на языке Java.
Чтобы использовать его для DDT нам нужно будет "параметризировать" класс с тестами.

    import org.openqa.selenium.firefox.FirefoxDriver;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.By;
    import org.junit.*;
    import org.junit.runner.RunWith;
    import org.junit.runners.Parameterized.Parameters;
    import org.junit.runners.Parameterized;
    import static org.junit.Assert.*;
    import java.util.Arrays;
    import java.util.Collection;
    
    @RunWith(value = Parameterized.class)
    public class SimpleDDT {
        private static WebDriver driver;
        private static StringBuffer verificationErrors = new
        StringBuffer();
        
        private String height;
        private String weight;
        private String bmi;
        private String bmiCategory;
        
        @Parameters
        public static Collection testData() {
            return Arrays.asList(
                new Object[][] {
                    {"160","45","17.6","Underweight"},
                    {"168","70","24.8","Normal"},
                    {"181","89","27.2","Overweight"},
                    {"178","100","31.6","Obesity"}
            }
        );
        
        public SimpleDDT(String height, String weight, String bmi, String bmiCategory)
        {
            this.height = height;
            this.weight = weight;
            this.bmi = bmi;
            this.bmiCategory = bmiCategory;
        }
        
        @Test
        public void testBMICalculator() throws Exception {
            //Get the Height element and set the value using parameterised
            //height variable
            WebElement heightField = driver.findElement(By.name("heightCMS"));
            heightField.clear();
            heightField.sendKeys(height);
            
            //Get the Weight element and set the value using parameterised
            //Weight variable
            WebElement weightField = driver.findElement(By.name("weightKg"));
            weightField.clear();
            weightField.sendKeys(weight);
            
            //Click on Calculate Button
             WebElement calculateButton = driver.findElement(By.id("Calculate"));
            calculateButton.click();
            try {
                //Get the Bmi element and verify its value using parameterised
                //bmi variable
                WebElement bmiLabel = driver.findElement(By.name("bmi"));
                assertEquals(bmi, bmiLabel.getAttribute("value"));
                
                //Get the Bmi Category element and verify its value using
                //parameterised bmiCategory variable
                WebElement bmiCategoryLabel = driver.findElement(By.name("bmi_category"));
                assertEquals(bmiCategory,bmiCategoryLabel.
                getAttribute("value"));
                
            } catch (Error e) {
                //Capture and append Exceptions/Errors
                verificationErrors.append(e.toString());
                System.err.println("Assertion Fail "+ verificationErrors.
                toString());
            }
        }
     }

## TestNG


TestNG — это фреймворк для тестирования, написанный Java, он взял много чего с JUnit и NUnit, но он не только унаследовался от существующей функциональности Junit, а также внедрения новых инновационных функций, которые делают его мощным, простым в использовании.
Попробуем применить его:

    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.firefox.FirefoxDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.By;
    import org.testng.annotations.*;
    import static org.testng.Assert.*;
    
    public class TestNGDDT {
        private WebDriver driver;
        private StringBuffer verificationErrors = new StringBuffer();
        
        @DataProvider
            public Object[][] testData() {
                return new Object[][] {
                    new Object[] {"160","45","17.6","Underweight"},
                    new Object[] {"168","70","24.8","Normal"},
                    new Object[] {"181","89","27.2","Overweight"},
                    new Object[] {"178","100","31.6","Obesity"},
            };
        }
        
        @BeforeTest
        public void setUp() {
            // Create a new instance of the Firefox driver
            driver = new FirefoxDriver();
            driver.get("http://dl.dropbox.com/u/55228056/bmicalculator.html");
        }
        
        @Test(dataProvider = "testData")
        public void testBMICalculator(String height, String weight, String
        bmi, String category) {
            try {
                WebElement heightField = driver.findElement(By.name("heightCMS"));
                heightField.clear();
                heightField.sendKeys(height);
                
                WebElement weightField = driver.findElement(By.name("weightKg"));
                weightField.clear();
                weightField.sendKeys(weight);
                
                WebElement calculateButton = driver.findElement(By.id("Calculate"));
                calculateButton.click();
                
                WebElement bmiLabel = driver.findElement(By.name("bmi"));
                assertEquals(bmiLabel.getAttribute("value"),bmi);
                
                WebElement bmiCategoryLabel = driver.findElement(By.name("bmi_category"));
                assertEquals(bmiCategoryLabel.getAttribute("value"),category);
                
            } catch (Error e) {
                //Capture and append Exceptions/Errors
                verificationErrors.append(e.toString());
            }
        }
        
        @AfterTest
        public void tearDown() {
            //Close the browser
            driver.quit();
            String verificationErrorString = verificationErrors.toString();
            if (!"".equals(verificationErrorString)) {
                fail(verificationErrorString);
            }
        }
    }

