# Cucumber JVM + Selenium Webdriver.

**Cucumber JVM** - это один из популярных инструментов реализации Behavior Driven Development (BDD) подхода в Java. Этот инструмент позволяет создавать тетсы любому участнику проектной команды. Для этого используют язык Gherkin, в котором основными являются следующие слова: Given, When и Then. Тесты, созданные таким образом, храняться в файлах с расширением ".feature".


Зависимости для Maven проекта:

    <project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://
    maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>FundTransfer</groupId>
        <artifactId>FundTransfer</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <dependencies>
            <dependency>
                <groupId>info.cukes</groupId>
                <artifactId>cucumber-java</artifactId>
                <version>1.0.14</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>info.cukes</groupId>
                <artifactId>cucumber-junit</artifactId>
                <version>1.0.14</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.10</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>org.seleniumhq.selenium</groupId>
                <artifactId>selenium-java</artifactId>
                <version>2.25.0</version>
            </dependency>
        </dependencies>
    </project>
    
    
Сценарий написанный с помощью языка Gherkin:

    Feature: Customer Transfer's Fund
             As a customer,
             I want to transfer funds
             so that I can send money to my friends and family
    Scenario: Valid Payee
              Given the user is on Fund Transfer Page
              When he enters "Jim" as payee name
              And he enters "100" as amount
              And he Submits request for Fund Transfer
              Then ensure the fund transfer is complete with "$100
              transferred successfully to Jim!!" message
    Scenario: Invalid Payee
              Given the user is on Fund Transfer Page
              When he enters "Jack" as payee name
              And he enters "100" as amount
              And he Submits request for Fund Transfer
              Then ensure a transaction failure message "Transfer
              failed!! 'Jack' is not registered in your List of Payees"
              is displayed
    Scenario: Account is overdrawn past the overdraft limit
              Given the user is on Fund Transfer Page
              When he enters "Tim" as payee name
              And he enters "1000000" as amount
              And he Submits request for Fund Transfer
              Then ensure a transaction failure message "Transfer
              failed!! account cannot be overdrawn" is displayed
              
Создание java класса:

    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.chrome.ChromeDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.By;
    import cucumber.annotation.*;
    import cucumber.annotation.en.*;
    import static org.junit.Assert.assertEquals;
    
    public class FundTransferStepDefs {
        protected WebDriver driver;
        
        @Before
        public void setUp() {
            driver = new ChromeDriver();
        }
        
        @Given("the user is on Fund Transfer Page")
        public void The_user_is_on_fund_transfer_page() {
            driver.get("http://dl.dropbox.com/u/55228056/fundTransfer.html");
        }
        
        @When("he enters \"([^\"]*)\" as payee name")
        public void He_enters_payee_name(String payeeName) {
            driver.findElement(By.id("payee")).sendKeys(payeeName);
        }
        
        @And("he enters \"([^\"]*)\" as amount")
        public void He_enters_amount(String amount) {
            driver.findElement(By.id("amount")).sendKeys(amount);
        }
        
        @And("he Submits request for Fund Transfer")
        public void He_submits_request_for_fund_transfer() {
            driver.findElement(By.id("transfer")).click();
        }
        
        @Then("ensure the fund transfer is complete with \"([^\"]*)\"\ message")
        public void Ensure_the_fund_transfer_is_complete(String msg) {
            WebElement message = driver.findElement(By.id("message"));
            assertEquals(message.getText(),msg);
        }
        
        @Then("ensure a transaction failure message \"([^\"]*)\" is displayed")
        public void Ensure_a_transaction_failure_message(String msg) {
            WebElement message = driver.findElement(By.id("message"));
            assertEquals(message.getText(),msg);
        }
        
        @After
        public void tearDown() {
            driver.close();
        }
    }
    
И для изменения настроек Cucumber-JVM мы добавим класс конфигурации:

    package fundtransfer.test;
    
    import cucumber.junit.Cucumber;
    import org.junit.runner.RunWith;
    
    @RunWith(Cucumber.class)
    @Cucumber.Options(format = {"pretty", "html:target/cucumber-htmlreport",
    "json-pretty:target/cucumber-report.json"})
    public class RunCukesTest {
        
    }
