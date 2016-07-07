# Автоматизация Canvas элементов.

Одним из нововведений HTML 5 был тег < canvas >, с помощью которого можно было создавать различные графики и рисунки используя JavaScript. Многие разарботчики стали активно его применять при создании веб-приложений. Поэтому любой автоматизатор может легко оказаться на проекте, где нужно будет автоматизировать работу и с Canvas элементами. Так давайте поробуем с помощью Selenium WebDiver это сделать:

    @Test
    public void testHTML5CanvasDrawing() throws Exception {
        //Get the HTML5 Canvas Element
        WebElement canvas = driver.findElement(By.id("imageTemp"));
        
        //Select the Pencil Tool
        Select drawtool = new Select(driver.findElement(By.id("dtool")));
        drawtool.selectByValue("pencil");
        
        //Create a Action Chain for Draw a shape on Canvas
        Actions builder = new Actions(driver);
        builder.clickAndHold(canvas).moveByOffset(10, 50).
                                        moveByOffset(50,10).
                                        moveByOffset(-10,-50).
                                        moveByOffset(-50,-10).release().perform();
                                        
        //Get a screenshot of Canvas element after Drawing and
        //compare it to the base version to verify if the Drawing is performed
        FileUtils.copyFile(WebElementExtender.captureElementBitmap(canvas), new File("c:\\tmp\\post.png"));
        assertEquals(CompareUtil.Result.Matched, CompareUtil.CompareImage("c:\\tmp\\base_post.png", "c:\\tmp\\post.png"));
    }