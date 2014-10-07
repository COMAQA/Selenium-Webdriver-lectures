# Запуск тестов на десктоп и мобильных браузерах

Для запуска тестов с помощью Appium нужно в начале настроить драйвер, а для этого нужно ему выставить правильные свойства.

Для запуска тестов под Android свойства драйвера будут следующие:

    public class AndroidTest {
    
        private WebDriver driver;
    
        @Before
        public void setUp() throws Exception {
            File classpathRoot = new File(System.getProperty("user.dir"));
            File appDir = new File(classpathRoot, "../../../apps/ApiDemos/bin");
            File app = new File(appDir, "ApiDemos-debug.apk");
            DesiredCapabilities capabilities = new DesiredCapabilities();
            capabilities.setCapability("device","Android");
            capabilities.setCapability(CapabilityType.BROWSER_NAME, "");
            capabilities.setCapability(CapabilityType.VERSION, "4.2");
            capabilities.setCapability(CapabilityType.PLATFORM, "MAC");
            capabilities.setCapability("app", app.getAbsolutePath());
            capabilities.setCapability("app-package", "com.example.android.apis");
            capabilities.setCapability("app-activity", ".ApiDemos");
            driver = new SwipeableWebDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
        }
     
        //Other Test methods...   
    }
    
Если же мы хотим тесты на мобильнос Safari, то:

     @Before
        public void setUp() throws Exception {
            DesiredCapabilities capabilities = new DesiredCapabilities();
            capabilities.setCapability("device", "iPhone Simulator");
            capabilities.setCapability("version", "6.1");
            capabilities.setCapability("app", "safari");
            driver = new RemoteWebDriver(new URL("http://127.0.0.1:4723/wd/hub"),
                    capabilities);
            driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
        }