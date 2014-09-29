# Альтернативные Page Object подходы.

## Page Object и Thucydides

Одним из альтернативных подходов является использование фреймворка Thucydides. Он переводится как Фукиди́д , который был древнегреческим историком и полководцем, который во время Пелопоннесской войны прославился именно качественными репортами, отсюда и название этого фреймворка.

Thucydides — это бесплатный проект с открытым исходным кодом. Разрабатываемый с 2011 года. Проект постоянно обновляется по мере добавления новой функциональности в новых версиях Selenium.

Фреймворк реализует Page Object паттерн и позволяет уменьшить дублирование кода за счет использования еще одного типа классов между тестами и страницами, так называемых «степов».

Посмотрим, как выглядят класс "степов" и page object класс:

    public class StepsinBook extends ScenarioSteps {
        public StepsinBook(Pages pages) {
            super(pages);
        }
        public BookPage getPageBook()
        {
            return  getPages().currentPageAt(BookPage.class);
        } 
        @Step
        public void getMain(String url)
        {
            getPageBook().getMainPage(url);
        } 
        @Step
        public void AllBooks()
        {
            getPageBook().allBooks();
        } 
        @Step
        public void search(){
            getPageBook().search("Книга");
        }
        @Step
        public void catalog(){
            getPageBook().catalog();
        }
    }
    
    public class BookPage extends PageObject {
        @FindBy(linkText = "Все книги")
        WebElement allbooksButton;
    
        @FindBy(linkText = "Поиск")
        WebElement searchButton;
    
        @FindBy(name = "query")
        WebElement searchField;
    
        @FindBy(css = "button")
        WebElement searchBegin;
        
        public BookPage(WebDriver driver) {
            super(driver);
        }
    
        public void getMainPage(String url) {
            getDriver().get(url);
    
        }
    
        public void allBooks() {
             allbooksButton.click();
        }
    
        public void search(String searchWord) {
            searchButton.click();
            searchField.sendKeys(searchWord);
            searchBegin.click();
    
        }

    }

Thucydides позволяет запускать тесты во всех браузерах, поддерживаемых Selenium, и полностью берет на себя работу с драйвером, его настройку, запуск и остановку. 


## Page Factory

Еще одним альтернативным подходом является использование класса Page Factory из библиотеки Selenium. Давайте разберёмся, как с тим работать. Сначала нам нужно создать простой page object:

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