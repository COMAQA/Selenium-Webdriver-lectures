# Инструменты для тестирования производительности
Существует множество различных инструментов для тестирования производительности веб приложений. Среди наиболее популярных можно назвать:
* DynaTrace <br>
http://www.compuware.com/en_us/application-performance-management/products/dynatrace-free-trial.html
* HttpWatch <br>
http://www.httpwatch.com

###DynaTrace###
Compuware DynaTrace предоставляет довольно широкий перечень платных услуг для тестирования производительности приложений. Полный список можно найти на официальном сайте, указанном выше.

Для наших целей (автоматизация) нужен инструмент dynaTrace AJAX Edition, который можно использовать при автоматизации в частности вместе с Selenium Webdriver. Загрузить инструмент, а также его подробное описание можно найти по адресу http://www.compuware.com/en_us/application-performance-management/products/ajax-free-edition/overview.html

DynaTrace AJAX Edition работает в браузерах:
* Internet Explorer 8-11
* Firefox 17-30

На платформах:
* Windows Vista
* Windows 7
* Windows 8, 8.1

На ряду с бесплатной версией можно купить premium edition, которая обладает расширенным функционалом.

При установке DynaTrace автоматически установит эддоны для IE и Firefox. После окончания установки следует убедиться, что эддоны включены.

При запуске тестов необходимо:
1. Запустить dynaTrace AJAX Edition перед запуском тестов.
2. Установить глобальные переменные окружения:
```
SET DT_AE_AGENTACTIVE=true
SET DT_AE-AGENTNAME=Firefox
```
Это можно сделать как в IntelliJ Idea (run => edit configurations => Environment variables => "+"), если вы запускаете тесты через IntelliJ Idea, или, используя команды для командной строки, указанные выше, если вы запускаете тесты через командную строку (например, с помощью maven), или же просто установив переменные вручную в windows.
3. Использовать браузер с профилем по умолчанию (default):
```
@Before
public void setUp() throws Exception
{
        // Создаем экземпляр профиля и получаем профиль "по умолчанию"
        ProfilesIni profile = new ProfilesIni();
        FirefoxProfile ffprofile = profile.getProfile("default");
        // Создаем драйвер, передав ему профиль "по умолчанию"
        driver = new FirefoxDriver(ffprofile);
}
```
В результате запуска тестов в окне dynaTrace AJAX Edition под опцией Session вы увидите данные о производительности, собранные в ходе теста, а также рекомендации по оптимизации производительности приложения.
![](/resources/dynatrace_report.png)
