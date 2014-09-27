# WebDriver. Обзор и принцип работы

Selenium Webdriver - инструмент для автоматизации реального браузера, как локально, так и удаленно, наиболее близко имитирующий действия пользователя.

По сравнению с Selenium RC Webdriver использует совершенно иной способ взаимодействия с браузеров. Он напрямую вызывает команды браузера, используя родной для каждого конкретного браузера API. Как совершаются эти вызовы и какие функции они выполняют зависит от конкретного браузера. В то время как Selenium RC внедрял javascript код в браузер при запуске и использовал его для управления веб приложением. Таким образом, Webdriver использует способ взаимодействия с браузером, более близкий к действиям реального пользователя.

Selenium 1.0 (RC) + WebDriver = Selenium 2.0 
* Интерфейс Webdriver был спроектирован более простым и выразительным;
* Webdriver обладает более компактным и объектно-ориентированным API;
* Webdriver управляет браузером более эффективно, а также справляется с некоторыми ограничениями, характерными для Selenium RC, как загрузка и отправление файлов, попапы и дилоги.