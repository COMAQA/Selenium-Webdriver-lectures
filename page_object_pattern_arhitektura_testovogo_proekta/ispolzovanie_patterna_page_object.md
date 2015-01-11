# Использование паттерна Page Object.

## Page object

Page Object - один из наиболее полезных и используемых архитектурных решений в автоматизации. Данный шаблон проектирования помогает инкапсулировать работу с отдельными элементами страницы, что позволяет уменьшить количество кода и упростить его поддержку. Если, к примеру, дизайн одной из страниц изменён, то нам нужно будет переписать только соответствующий класс, описывающий эту страницу.

Основные преимущества:

* Разделение кода тестов и описания страниц
* Объединение всех действий по работе с веб-странией в одном месте

Давайте посмотрим, как с этим работать. Для примера возьмём страницу логина, которая есть практически на всех сайтах. Создадим класс с описание этой страницы, используя шаблон Page Object:

    public class LoginPage {
        By usernameLocator = By.id("username");
        By passwordLocator = By.id("passwd");
        By loginButtonLocator = By.id("login");
    
        private final WebDriver driver;
        
        public LoginPage(WebDriver driver) {
            this.driver = driver;
    
            if (!"Login".equals(driver.getTitle())) {
                throw new IllegalStateException("This is not the login page");
            }
        }
    
        public LoginPage typeUsername(String username) {
            driver.findElement(usernameLocator).sendKeys(username);
            return this;    
        }
    
        public LoginPage typePassword(String password) {
            driver.findElement(passwordLocator).sendKeys(password);
            return this;    
        }
    
        public HomePage submitLogin() {
            driver.findElement(loginButtonLocator).submit();
            return new HomePage(driver);    
        }
    
        public LoginPage submitLoginExpectingFailure() {
            driver.findElement(loginButtonLocator).submit();
            return new LoginPage(driver);   
        }
    
        public HomePage loginAs(String username, String password) {
            typeUsername(username);
            typePassword(password);
            return submitLogin();
        }
}
    
    
## Page Factory

Еще одним вариантом применения page object шаблона является использование класса Page Factory из библиотеки Selenium. Давайте разберёмся, как с ним работать. Сначала нам нужно создать простой page object:

    public class GoogleSearchPage {
        private WebElement q;
    
        public void searchFor(String text) {
            q.sendKeys(text);
            q.submit();
        }
    } 
    
Теперь чтобы всё работало корректно нам нужно инициализировать наш page object. Это выглядит так:

    package org.openqa.selenium.example;
    
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.htmlunit.HtmlUnitDriver;
    import org.openqa.selenium.support.PageFactory;
    
    public class UsingGoogleSearchPage {
        public static void main(String[] args) {
            WebDriver driver = new HtmlUnitDriver();
    
            driver.get("http://www.google.com/");
    
            GoogleSearchPage page = PageFactory.initElements(driver, GoogleSearchPage.class);
    
            page.searchFor("Cheese");
        }
    }     