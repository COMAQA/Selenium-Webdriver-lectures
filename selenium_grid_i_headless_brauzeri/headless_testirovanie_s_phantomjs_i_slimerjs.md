# "Headless" тестирование с PhantomJS и SlimerJS

##PhantomJS

**PhantomJS** — это сборка движка WebKit без графического интерфейса, позволяющая в режиме консоли загружать веб-страницу, выполнять JavaScript, полноценно работать с DOM, Canvas, CSS, JSON и SVG. WebKit лежит в основе таких популярных браузеров как Chrome и Safari. Вы имеете возможность интеграции с различными фреймворками для тестирования JavaScript и веб-страниц начиная от Jasmine до WebDriver. Для работы с PhantomJS браузером был реализван драйвер на чистом JavaScript, который получил название GhostDriver. Вот пример теста, в котором он импользуется:


    package ghostdriver;

    import org.junit.Test;
    import org.openqa.selenium.By;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.phantomjs.PhantomJSDriver;

    import static junit.framework.TestCase.assertEquals;

    public class PhantomJSCommandTest extends BaseTest {
        @Test
        public void executePhantomJS() {
            WebDriver d = getDriver();
            if (!(d instanceof PhantomJSDriver)) {
                // Skip this test if not using PhantomJS.
                // The command under test is only available when using PhantomJS
                return;
            }

            PhantomJSDriver phantom = (PhantomJSDriver)d;

            // Do we get results back?
            Object result = phantom.executePhantomJS("return 1 + 1");
            assertEquals(new Long(2), (Long)result);

            // Can we read arguments?
            result = phantom.executePhantomJS("return arguments[0] + arguments[0]", new Long(1));
            assertEquals(new Long(2), (Long)result);

            // Can we override some browser JavaScript functions in the page context?
            result = phantom.executePhantomJS("var page = this;" +
               "page.onInitialized = function () { " +
                    "page.evaluate(function () { " +
                        "Math.random = function() { return 42 / 100 } " +
                    "})" +
                "}");

            phantom.get("http://ariya.github.com/js/random/");

            WebElement numbers = phantom.findElement(By.id("numbers"));
            boolean foundAtLeastOne = false;
            for(String number : numbers.getText().split(" ")) {
                foundAtLeastOne = true;
                assertEquals("42", number);
            }
            assert(foundAtLeastOne);
        }
    }



Официальный сайт: http://phantomjs.org/

Репозиторий с Ghost Driver: https://github.com/detro/ghostdriver

##SlimerJS

**SlimerJS** - это скриптовый браузер для разработчика имеющий в своем арсенале движок эквивалентный последнему Firefox(Gecko), который лежит в основе ещё нескольких браузеров. Его также можно использовать вместе с GhostDriver, благодаря своей схожести с PhantomJS.


Официальный сайт: http://slimerjs.org/

Репозиторий на GitHub: https://github.com/laurentj/slimerjs
