# Альтернативные Page Object подходы.

В предыдущей главе мы рассмотрели как использовать паттерн Pae Object. Мы сделали нашу собсвенную реализацию. Однако данный шаблон очень популярен, поэтому в некоторых языках его применение уже реализовано (например Java или Ruby). Так давайте же посмотрим как это сделано. Для применра возьмем реалтзацию на языке Java. Паттерн Page Object в Selenium реализован с помощью библиотеки PageFactory и класса страницы.

Результатом такого подхода является то, что мы должны моделировать как успешные, так и неуспешные исходы теста. Например, в случае если при использовании элемента может совершаться переход на различные страницы необходимо создавать разные методы для каждого отдельного случая.

Поиск элементов помно делать двумя способами используя:

* driver.findElement
* аннотацию @FindBy

Аннотация @FindBy работает только с использованием PageFactory. Без вызова PageFactory.initElements(driver, this); при обращении к элементам Вы получите NullPointerException.

Вот как выглядит пример с использованием PageFactory: 

взял тут: http://internetka.in.ua/selenium-page-object/

    import org.junit.Assert;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.support.FindBy;
    import org.openqa.selenium.support.PageFactory;
     
    public class RegistrationPage {
     
        private static String URL_MATCH = "registration";
     
        private WebDriver driver;
     
        /**
         * Логин
         */
        @FindBy(id = "userLogin")
        private WebElement login;
     
        /**
         * Пароль
         */
        @FindBy(id = "regPassword")
        private WebElement password;
     
        /**
         * Пароль подтверждение
         */
        @FindBy(id = "passwordConfirmation")
        private WebElement passwordConfirm;
     
        /**
         * E-mail
         */
        @FindBy(id = "UserEmail")
        private WebElement email;
     
        /**
         * Кнопка зарегистрировать
         */
        @FindBy(id = "submitRegistration")
        private WebElement bSubmitRegister;
     
        /**
         * Сообщение об ошибке
         */
        @FindBy(id = "error-message")
        private WebElement registerError;
     
        public RegistrationPage(WebDriver driver) {
            // проверить, что вы находитесь на верной странице
            if (!driver.getCurrentUrl().contains(URL_MATCH)) {
                        throw new IllegalStateException(
                            "This is not the page you are expected"
                            );
            }
     
            PageFactory.initElements(driver, this);
            this.driver = driver;
        }
     
        /**
         * Регистраци пользователя
         * @param user - {@link User}
         */
        private void registerUser(User user) {
            System.out.println(driver.getTitle());
            login.sendKeys(user.login);
            password.sendKeys(user.password);
            passwordConfirm.sendKeys(user.passwordConfirmation);
            email.sendKeys(user.email);
     
            bSubmitRegister.click();
        }
     
        /**
         * Успешная регистрация пользователя
         * @param user - {@link User}
         * @return {@link HomePage}
         */
        public HomePage registerUserSuccess(User user) {
            registerUser(user);
            return new HomePage(driver);
        }
     
        /**
         * Неуспешная регистрация
         * @param user - {@link User}
         * @return {@link RegistrationPage}
         */
        public RegistrationPage registerUserError(User user) {
            registerUser(user);
            return new RegistrationPage(driver);
        }
     
        /**
         * Проверить сообщение об ошибке
         * @param user - {@link User}
         * @return {@link RegistrationPage}
         */
        public RegistrationPage checkErrorMessage(String errorMessage) {
            Assert.assertTrue("Error message should be present", 
                            registerError.isDisplayed());
            Assert.assertTrue("Error message should contains " + errorMessage, 
                            registerError.getText().contains(errorMessage));
            return this;
        }
    }
    

Возможно можно будет добавить вот это:
http://internetka.in.ua/selenium-fielddecorator/