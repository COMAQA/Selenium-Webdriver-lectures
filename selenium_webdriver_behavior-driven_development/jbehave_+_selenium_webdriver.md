# JBehave + Selenium Webdriver.

**JBehave** – это еще один из инструментов пользующихся популярностью, когда речь заходит о BDD подходе на проекте. Он похож на Cucumber-JVM, так же использует язык Gherkin для написания тестовых сценариев. Только расширение для файлов здесть уже ".story". Вот пример такого файла:

    Narrative: I should be able to Calculate my Body Mass Index
    
    Scenario: I should see my BMI after entering Height and Weight
    
    When I open BMI Calculator Home Page
    When I enter height as '181'
    When I enter weight as '80'
    When I click on the Calculate button
    Then I should see bmi as '24.4' and category as 'Normal'

Следующим шагом, после создания тестового сценария, является написание java класса, котрый может выглядеть следующим образом:

    import junit.framework.Assert;
    import org.jbehave.core.annotations.Then;
    import org.jbehave.core.annotations.When;
    
    import org.openqa.selenium.By;
    import org.openqa.selenium.WebElement;
    
    public class Bmi extends StoryBase {
        
        @When("I open BMI Calculator Home Page")
        public void IOpen(){
            driver.get("http://dl.dropbox.com/u/55228056/bmicalculator.html");
        }
        
        @When("I enter height as '$height'")
        public void IEnterHeight(String height){
            WebElement heightCMS = driver.findElement(By.id("heightCMS"));
            heightCMS.sendKeys(height);
        }
        
        @When("I enter weight as '$weight'")
        public void IEnterWeight(String weight){
            WebElement weightKg = driver.findElement(By.id("weightKg"));
            weightKg.sendKeys(weight);
        }
        
        @When("I click on the Calculate button")
        public void IClickOnTheButton(){
            WebElement button = driver.findElement(By.id("Calculate"));
            button.click();
        }
        
        @Then("I should see bmi as '$bmi_exp' and category as '$bmi_category_exp'")
        public void IShouldBmiAndCategory(String bmi_exp, String bmi_category_exp){
            WebElement bmi = driver.findElement(By.id("bmi"));
            Assert.assertEquals(bmi_exp, bmi.getAttribute("value"));
            WebElement bmi_category = driver.findElement(By.id("bmi_category"));
            Assert.assertEquals(bmi_category_exp, bmi_category.getAttribute("value"));
            driver.quit();
        }
    }
    
После чего мы должны создать конфигурацинный файл для коректного взаимодействия Selenium и JBehave:

    import java.util.List;
    import org.jbehave.core.configuration.Configuration;
    import org.jbehave.core.configuration.MostUsefulConfiguration;
    import org.jbehave.core.io.LoadFromClasspath;
    import org.jbehave.core.junit.JUnitStory;
    import org.jbehave.core.reporters.Format;
    import org.jbehave.core.reporters.StoryReporterBuilder;
    import org.jbehave.core.steps.InstanceStepsFactory;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.firefox.FirefoxDriver;
    
    public abstract class StoryBase extends JUnitStory {
        
        protected final static WebDriver driver = new FirefoxDriver();
        
        @Override
        public Configuration configuration() {
            return new MostUsefulConfiguration()
            .useStoryLoader(new LoadFromClasspath(this.getClass().getClassLoader()))
            .useStoryReporterBuilder(new StoryReporterBuilder()
            .withDefaultFormats()
            .withFormats(Format.HTML,Format.CONSOLE)
            .withRelativeDirectory("jbehave-report")
        }
        
        @Override
        public List candidateSteps() {
            return new InstanceStepsFactory(configuration(), this).createCandidateSteps();
        }
    }