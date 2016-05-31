# Настройка тестов для запуска в SauceLabs.

Для запуска уже готовых автотестов с использованием Sauce Labs не требуется модификация самих тестовых скриптов. Достаточно внести изменения в настройку WebDriver:

```
caps.setCapability(CapabilityType.BROWSER_NAME, "Internet explorer");
caps.setCapability(CapabilityType.VERSION, "11");
caps.setCapability(CapabilityType.PLATFORM, "windows 7");
caps.setCapability(CapabilityType.PROXY, proxy.seleniumProxy());

WebDriver driver = new RemoteWebDriver(new
URL("http://vadimzubovich:cc729a89-a8ad-417e-aa0f-e601ab1105df@ondemand.saucelabs.com:80/wd/hub"),
caps);*/```

Готовые Desired Capabilities можно сгенерировать на сайте SauceLabs, в разделе Docs, в секции Platform Configurator: [Конфигуратор можно посмотреть тут.](https://wiki.saucelabs.com/display/DOCS/Platform+Configurator#/) Там можно выбрать необходимый инструмент (Selenium или Appium), устройство, платформу, браузер и прочие настройки. После этого скопировать сгенерированный код на одном из поддерживаемых языков программирования (Java, PHP, Node.js, Python, Ruby, C#) и вставить в свой проект при конфигурировании Веб-Драйвера.

В строке инициализации драйвера в передаваемом аргументе URL необходимо задать Access Key, сгенерированный в личном кабинете Sauce Labs и передать в качестве Capability созданный ранее экземпляр этого класса с указанием настроек, как показано в примере.