# Использование паттерна Page Object.

Page Object - один из наиболее полезных и используемых архитектурных решений в автоматизации. Данный шаблон проектирвоания помогает инкапсулировать работу с отдельными элементами страницы, что позволяет уменьшить количество кода и его поддержку. Если, к примеру, дизайн одной из страниц изменён, то нам нужно будет переписать только соотвестующий класс, описывающий эту страницу.

Основные преимущества:

* Разделение кода тестов и описания страниц
* Объединение всех действий по работе с веб-странией в одном месте

Давайте посмотрим, как с этим работать. Для примера возьмём страницу логина, которая есть практически на всех сайтах. Сначала создадим класс с описание этой страницы, используя шаблон Page Object:


    public class LoginPage extends PageBase {
        private static final String TITLE = "Вход в систему";
    
        private static final By FORM = get("loginPage.form");
        private static final By NAME_FIELD = get("loginPage.userNameInput");
        private static final By PASS_FIELD = get("loginPage.userPassInput");
        private static final By LOGIN = get("loginPage.loginButton");
    
        public static final By ERROR_MESSAGE = get("loginPage.errorMessage");
    
        public static void login(String name, String pass){
            $(NAME_FIELD).val(name);
            $(PASS_FIELD).val(pass);
            $(LOGIN).click();
        }
        
    }    
    
А теперь используем его в тесте:

    public class LoginTest extends TestBase {
        private static final String ERROR_MSG = "Неверный пароль или имя пользователя";
        private static final String TITLE = "Инфопанель";

        public void loginTest(String user, String pass, String isValid) {
            LoginPage.login(user, pass);
            checkLoginResult(isValid);
        }
        
        private void checkLoginResult(String isValid) {
        if(Boolean.parseBoolean(isValid)){
            Waiter.waitForPageTitle(TITLE);
        }
        else {
            $(LoginPage.ERROR_MESSAGE).shouldBe(visible).shouldHave(text(ERROR_MSG));
        }
    }
    }
    
    