# Работа с web storage.

Cookie-файлы используются с самого начала применения JavaScript, поэтому хранение данных в веб-формате не является чем.то инновационным. Однако Web Storage является куда более мощным инструментом для хранения данных, так как обеспечивает больший уровень безопасности, повышенную скорость и простоту использования. Кроме того, в Web Storage можно хранить большие объемы данных. Ограничения определяются конкретным браузером, но обычно допускается использование от 5 до 10 Мбайт, что более чем достаточно для HTML-приложений. Еще одно преимущество — данные не загружаются с каждым запросом на сервер. Впрочем, существует одно ограничение – нельзя обобществлять веб-хранилища между браузерами: если вы сохранили данные в Safari, они будут недоступны Mozilla Firefox.

Теперь же посмотрим, что мы можем сдеаль с этим хранилищем данных, испотльзуя возможности WebDriver.

    @Test
    public void testHTML5LocalStorage() throws Exception {
        String lastName;
        JavascriptExecutor jsExecutor = (JavascriptExecutor) driver;
        
        //Get the current value of localStorage.lastname, this should be Smith
        lastName = (String) jsExecutor.executeScript("return localStorage.lastname;");
        assertEquals("Smith", lastName);
    }