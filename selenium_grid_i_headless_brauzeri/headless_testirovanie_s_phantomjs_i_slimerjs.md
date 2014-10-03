# "Headless" тестирование с PhantomJS и SlimerJS

##PhantomJS

**PhantomJS** — это сборка движка WebKit без графического интерфейса, позволяющая в режиме консоли загружать веб-страницу, выполнять JavaScript, полноценно работать с DOM, Canvas, CSS, JSON и SVG. WebKit лежит в основе таких популярных браузеров как Chrome и Safari. Вы имеете возможность интеграции с различными фреймворками для тестирования JavaScript и веб-страниц начиная от Jasmine до WebDriver.


Пример:

    //create new webpage object
    var page = require('webpage').create();
    
    page.viewportSize = { width: 600, height: 600 };
    
    //load the page
    page.open('http://dou.ua/', function (status) {
        if (status !== 'success') {
            console.log('Unable to load the address: ' + status);
            phantom.exit();
        } else {
            window.setTimeout(function () {
                page.render('dou.png');
                phantom.exit();
            }, 200);
        }
    });


Официальный сайт: http://phantomjs.org/


##SlimerJS



##CasperJS

Правилом хорошего тона при работе с PhantomJS является испоьлзование CasperJS
CasperJS – вспомогательный инструмент написанный на JavaScript как обертка PhantomJS.

На официальном сайте перечислены следующие основные возможности:

* Определение и порядок итераций браузера( defining & ordering browsing navigation steps)
* Заполнение и отправка форм
* Клик и переход по ссылкам
* Создание скриншотов страницы и ее части
* Удаленное тестирование DOM
* Логирование событий
* Загрузка ресурсов и подключение библиотек
* Написание функциональных тестов и сохранение в формате JUnit XML
* Допиливание веб контента