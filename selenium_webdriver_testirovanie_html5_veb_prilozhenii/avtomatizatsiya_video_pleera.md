# Автоматизация видео плеера.

Ранее, чтобы посмотреть видео в интернете, нужно было использовать различные плагины. При этом не существовало чего-то универсального. Однако, после выхода HTML 5 ситуация изменилась. Появился тег < video >, с помощью которого можно было добавлять легко видео на сайт. Соответственно, тестировать его тоже нужно.
Для этого будем использовать класс JavaScriptExecutor. Напишем простой тест и посмотрим, как это работает:

    @Test
    public void testHTML5VideoPlayer() throws Exception {
        File scrFile = null;
        
        //Get the HTML5 Video Element
        WebElement videoPlayer = driver.findElement(By.id("vplayer"));
        
        //We will need a JavaScript Executor for interacting
        //with Video Element's
        //methods and properties for automation
        JavascriptExecutor jsExecutor = (JavascriptExecutor) driver;
        
        //Get the Source of Video that will be played in Video Player
        String source = (String) jsExecutor.executeScript("return arguments[0].currentSrc;", videoPlayer);
        
        //Get the Duration of Video
        long duration = (Long) jsExecutor.executeScript("return arguments[0].duration", videoPlayer);
        System.out.println(duration);
        
        //Verify Correct Video is loaded and duration
        assertEquals("http://html5demos.com/assets/dizzy.mp4", source);
        assertEquals(25, duration);
        
        //Play the Video
        jsExecutor.executeScript("return arguments[0].play()", videoPlayer);
        Thread.sleep(5000);
        
        //Pause the video
        jsExecutor.executeScript("arguments[0].pause()", videoPlayer);
        
        //Take a screen-shot for later verification
        scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
        FileUtils.copyFile(scrFile, new File("c:\\tmp\\pause_play.png"));
    }