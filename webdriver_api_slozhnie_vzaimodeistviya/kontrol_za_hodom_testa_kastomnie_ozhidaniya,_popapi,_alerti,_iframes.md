# Контроль за ходом теста. Кастомные ожидания, попапы, алерты, Iframes.

## Ожидания


Selenium WebDriwer имеет хороший набор стандартных "ожидалок"(waiters), но бывают случаи, когда их не достаточно. В этом случае мы можем написать собсвенную "ожидалку". В этом нам поможет класс ExpectedCondition.

Как же это работает? Давайте посмотрим на примере.

    @Test
    public void testExplicitWait()
    {
        //Go to Sample Application
        WebDriver driver = new FirefoxDriver();
        driver.get("http://dl.dropbox.com/u/55228056/AjaxDemo.html");
        try {
            //Get the link for Page 4 and click on it, this will call AJAX code
            //for loading the contents for Page 4
    
            WebElement page4button = driver.findElement(By.linkText("Page4"));
            page4button.click();
    
            //Create Wait using WebDriverWait.
            //This will wait for 5 seconds for timeout before page4 element is found
            //Element is found in specified time limit test will move to the text step
            //instead of waiting for 10 seconds
            //Expected condition is expecting a WebElement to be returned
            //after findElement finds the
            //element with specified locator
    
            WebElement message = (new WebDriverWait(driver, 5))
            .until(new ExpectedCondition<WebElement>(){
                @Override
                public WebElement apply(WebDriver d) {
                    return d.findElement(By.id("page4"));
                }});
            assertTrue(message.getText().contains("Nunc nibh tortor"));
        } catch (NoSuchElementException e) {
            fail("Element not found!!");
            e.printStackTrace();
        } finally {
            driver.close();
        }
    }
    
Например, если мы захотим что-то выполнить при появлении какого-либо элемента с использованием jQuery, то переопределяемый метод будет выглядеть так

    WebElement element = (new WebDriverWait(driver, 10)).until(new ExpectedCondition<Boolean>()
    {
        public Boolean apply(WebDriver d) {
            JavascriptExecutor js = (JavascriptExecutor) d;
            return (Boolean)js.executeScript("return jQuery.active == 0");
    }});


Еще одной распространенной задачей является работа с всплывающими окнами. Соотвественно нам нужно переключиться на pop-up выполнить какие-либо действия(ввод данных, проверки и т д), а затем вернуться на родительское окно. Чтобы лучше в этом разобраться выполним простой пример:

    @Test
    public void testWindowPopup()
    {
        //Save the WindowHandle of Parent Browser Window
        String parentWindowId = driver.getWindowHandle();
    
        //Clicking Help Button will open Help Page in a new Popup Browser Window
        WebElement helpButton = driver.findElement(By.id("helpbutton"));
        helpButton.click();
        try {
            //Switch to the Help Popup Browser Window
            driver.switchTo().window("HelpWindow");
    
            } catch (NoSuchWindowException e) {
                e.printStackTrace();
        }

        //Verify the driver context is in Help Popup Browser Window
        assertTrue(driver.getTitle().equals("Help"));
    
        //Close the Help Popup Window
        driver.close();
    
        //Move back to the Parent Browser Window
        driver.switchTo().window(parentWindowId);
    
        //Verify the driver context is in Parent Browser Window
        assertTrue(driver.getTitle().equals("Build my Car - Configuration"));
    }
    
    
## Работа с alert окнами

Проводя время в интернете вы не раз сталкивались с ситуациями, когда вы ошиблись при заполнении одного из полей или не выставили галочку где-нибудь, то появлялись окошки, тербовавшие все исправить. Вот сейчас мы попробуем научится их отлавливать и взаимодействовать с ними.

    @Test
    public void testSimpleAlert()
    {
        //Clicking button will show a simple Alert with OK Button
        WebElement button = driver.findElement(By.id("simple"));
        button.click();
    
        try {
            //Get the Alert
            Alert alert = driver.switchTo().alert();
    
            //Get the Text displayed on Alert using getText() method of Alert class
            String textOnAlert = alert.getText();
    
            //Click OK button, by calling accept() method of Alert Class
            alert.accept();
    
            //Verify Alert displayed correct message to user
            assertEquals("Hello! I am an alert box!",textOnAlert);
    
        } catch (NoAlertPresentException e) {
            e.printStackTrace();
        }
    }
    
## Фреймы

Иногда разработчики хотят на одной странице использовать несколько окон или подокон. В разметке таких страниц вы обязательно встретите следующие теги frameset или iframe. Соответственно автоматизаторам необходимо уметь рабоать с ними. Помогать находить Selenium элементы в нужном подокне. Попробуем их различить с помощью имени или id. Рассмотрим пример:

    @Test
    public void testFrameWithIdOrName()
    {
        //Activate the frame on left side using it's id attribute
        driver.switchTo().frame("left");
    
        //Get an element from the frame on left side and verify it's contents
        WebElement leftmsg = driver.findElement(By.tagName("p"));
        assertEquals("This is Left Frame", leftmsg.getText());
    
        //Activate the Page, this will move context from frame back to the Page
        driver.switchTo().defaultContent();
    
        //Activate the frame on right side using it's name attribute
        driver.switchTo().frame("right");
    
        //Get an element from the frame on right side and verify it's contents
        WebElement rightmsg = driver.findElement(By.tagName("p"));
        assertEquals("This is Right Frame", rightmsg.getText());
    
        //Activate the Page, this will move context from frame back to the Page
        driver.switchTo().defaultContent();
    }