# Selenide
Selenide - это "обертка" для Selenium Webdriver, ориентированная, как и Geb, на быструю и лаконичную автоматизацию тестирования, но написанная на Java. Преимущества этого инструмента:
* Более удобный и лаконичный API
* Доступ к Webdriver
* Селекторы в стиле JQuery
* Работа с AJAX элементами
* Автостарт и уничтожение браузера

Пример теста:
```
@Test
public void testLogin() {
  open("/login");
  $(By.name("user.name")).type("johny");
  $("#submit").click();
  $("#username").shouldHave(text("Hello, Johny!"));
}
```
Maven зависимость:
```
<dependency>
	<groupId>com.codeborne</groupId>
	<artifactId>selenide</artifactId>
	<version>2.10</version>
</dependency>
```
Официальный сайт: http://selenide.org/

