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


В передаваемом аргументе URL необходимо задать Access Key, сгенерированный в личном кабинете Sauce Labs и передать в качестве Capability созданный ранее экземпляр этого класса с указанием настроек, как показано в примере.
Для получения готового кода с настройками можно воспользоваться генератором настроек, который находится на сайте Sauce Labs в разделе Platforms. Для этого необходимо выбрать требуемую платформу и браузер, после чего в правой части экрана выбрать тестовую платформу (в нашем случае Selenium RC) и язык программирования, после чего на экране появится готовый код.