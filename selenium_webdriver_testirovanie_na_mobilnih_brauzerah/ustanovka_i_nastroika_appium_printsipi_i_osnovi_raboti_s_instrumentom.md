# Установка и настройка Appium. Принципы и основы работы с инструментом

**Appium** – инструмент автоматизации мобильных приложений, использующих Webdriver API. Appium – HTTP сервер, который создает и управляет сессиями Webdriver.

Если мы хотим использовать Appium на Windows, то нам нужно воспользоваться Appium.exe.

**Установка**:

1. С помощью [Appium.exe](https://github.com/appium/appium-dot-exe).
2. Используя NPM:
    * Устанавливаем NPM: C:\node> npm install express -g (C:\Node - место установки node.js)
    * Запускаем NPM и выполняем команду: $ npm install appium

**Настройка**:
1. Первым делом мы устанавливаем [Node.js](http://nodejs.org/download/) (выше 0.10 версии)
2. Затем устанавливаем [Android SDK](http://developer.android.com/sdk/index.html). С поддержкой API Level 17 или выше. Создаем переменную окружения ANDROID_HOME, куда добавляем пути к папкам tools и platform-tools.
3. Устанавливаем Java JDK и прописываем к нему пути в переменной JAVA_HOME.
4. Далее нам нужно установить [Apache Ant](http://ant.apache.org/bindownload.cgi). Так же добавляем путь к его папке в переменную окружения - PATH.
5. После чего устанавливаем [Apache Maven](http://maven.apache.org/download.cgi). Создаем две переменные M2HOME и M2, куда соответственно вписываем пути к папке Maven и паке bin, внутри неё.
