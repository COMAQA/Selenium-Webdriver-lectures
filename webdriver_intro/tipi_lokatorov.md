# Типы локаторов

Поскольку Webdriver - это инструмент для автоматизации веб приложений, то большая часть работы с ним это работа с веб элементами(WebElements). WebElements - ни что иное, как DOM объекты, находящиеся на веб странице. А для того, чтобы осуществлять какие-то действия над DOM объектами / веб елементами необходимо их точным образом определить(найти).

        WebElement element = driver.findElement(By.<Selector>);
Таким образом в Webdriver определяется нужный элемент. By - класс, содержащий статические методы для идентификации элементов:

1.By.id

Пример:
```
<div id="element">
     <p>some content</p>
</div>```

Поиск элемента:
```
 WebElement element = driver.findElement(By.id("element_id"));```

2.By.name

Пример:

```
<div name="element">
     <p>some content</p>
</div>```

Поиск элемента:
```
WebElement element = driver.findElement(By.name("element_name"));```

3.By.className

Пример:

```
<img class="logo">```

Поиск элемента:
```
WebElement element = driver.findElement(By.className("element_class"));```

4.By.TagName

Пример:

```
<div>
     <a class="logo" ref="...">...</a>
     <a class="support" ref="...">...</a>
</div>```


Поиск элемента:
```
List<WebElement> elements = driver.findElements(By.tagName("a"));```

5.By.LinkText

Пример:

```
<div>
     <a ref="...">text</a>
     <a ref="...">Another text</a>
</div>```

Поиск элемента:

```WebElement element = driver.findElement(By.linkText("text"));```

6.By.PartialLinkText

Поиск элемента:

```WebElement element = driver.findElement(By.partialLinkText("text"));```

7.By.cssSelector

```
<div class='main'>
     <p>text</p>
     <p>Another text</p>
</div>```


Поиск элемента:
```
WebElement element=driver.FindElement(By.cssSelector("div.main"));```

8.By.XPath

```
<div class='main'>
     <p>text</p>
     <p>Another text</p>
</div>```


Поиск элемента:
```
WebElement element = driver.findElement(By.xpath("//div[@class='main']"));```
