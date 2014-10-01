# WebDriver API. Сложные взаимодействия.
Selenium WebDriver позволяет имитировать действия пользователя, начиная от простых движений мыши до сложных, перетягивание объета. Все это позволяет реализовать класс **Actions**.
Так же разработчики позаботились о том, чтобы мы могли создавать цепочку действий, используя этот класс. Рассмотрим некоторые возможности на следующих примерах.

Двойной щелчёк на элементе:

    @Test
    public void testDoubleClick() throws Exception
    {
        WebDriver driver = new ChromeDriver();
        driver.get("http://dl.dropbox.com/u/55228056/DoubleClickDemo.html");
        WebElement message = driver.findElement(By.id("message"));
        
        //Verify color is Blue
        assertEquals("rgb(0, 0, 255)",
        message.getCssValue("background-color").toString());
        Actions builder = new Actions(driver);
        builder.doubleClick(message).build().perform();
        
        //Verify Color is Yellow
        assertEquals("rgb(255, 255, 0)",
        message.getCssValue("background-color").toString());
        driver.close();
    }
    
Перетягивание объекта:

    @Test
    public void testDragDrop() {
        driver.get("http://dl.dropbox.com/u/55228056/DragDropDemo.html");
        WebElement source = driver.findElement(By.id("draggable"));
        WebElement target = driver.findElement(By.id("droppable"));
        
        Actions builder = new Actions(driver);
        builder.dragAndDrop(source, target).perform();
        try
        {
            assertEquals("Dropped!", target.getText());
        } catch (Error e) {
            verificationErrors.append(e.toString());
        }
    }
    
####Другие полезные методы

Клик левой кнопкой мыши:

    click()
    click(WebElement onElement)

Клик с удержанием:

    clickAndHold()
    clickAndHold(WebElement onElement)
    
Правый клик:
    
    contextClick()
    contextClick(WebElement onElement)

Пример работы с контекстным меню:

    Actions builder = new Actions(driver);
    builder.contextClick(webElement).sendKeys(Keys.ARROW_DOWN).sendKeys(Keys.ARROW_DOWN).sendKeys(Keys.RETURN).build().perform();

Перетаскивание со смещением:

    dragAndDropBy(WebElement source, int xOffset, int yOffset)
    
Нажатие и удержание клавиши и дальнейшее ее отпускание:

    keyDown(Keys theKey) / keyDown(WebElement element, Keys key)
    keyUp(Keys theKey) / keyUp(WebElement element, Keys key)
    
Смещение мыши:

    moveByOffset(int xOffset, int yOffset)
    
Перемещение мыши на элемент:

    moveToElement(WebElement toElement)
    moveToElement(WebElement toElement, int xOffset, int yOffset)
    
Отпускание клавиши мыши:

    release()
    release(WebElement onElement)
    
    //Вариант:
    sendKeys(Keys.NULL)

Набор текста на клавиатуре:

    sendKeys(java.lang.CharSequence... keysToSend)
    sendKeys(WebElement element, java.lang.CharSequence... keysToSend)
    
Построение цепочки действий:

    build()
    
Выполнение построенной цепочки действий:

    perform()
