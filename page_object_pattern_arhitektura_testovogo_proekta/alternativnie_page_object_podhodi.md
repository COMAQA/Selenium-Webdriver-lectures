# Альтернативные Page Object подходы.

## Page Object и Thucydides

Одним из альтернативных подходов является использование фреймворка Thucydides. Он переводится как Фукиди́д , это имя древнегреческого историка и полководца, который во время Пелопоннесской войны прославился именно качественными репортами, отсюда и название этого фреймворка.

Thucydides — это бесплатный проект с открытым исходным кодом, разрабатываемый с 2011 года. Проект постоянно обновляется по мере добавления новой функциональности в новых версиях Selenium.

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

## HTML Elements

Фреймворк HTML Elements — инструмент для простой работы с элементами веб-страниц в тестах.

HTML Elements позволяет собирать page-объекты как конструктор. Из типизированных элементов вы можете собирать нужные вам блоки, которые можно объединять, комбинировать друг с другом и собирать из них page-объекты. Это значительно повышает степень переиспользования кода, делает его более читаемым и наглядным, а написание тестов — более простым.


Основной репозиторий: https://github.com/yandex-qatools/htmlelements.

Документация: https://github.com/yandex-qatools/htmlelements/blob/master/README.md.

Также документирован код проекта.

Примеры: https://github.com/yandex-qatools/htmlelements-examples.

