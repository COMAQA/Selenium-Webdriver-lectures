# Thucydides
Thucydides - это инструмент с открытым исходным кодом, ориентированный на эффективную автоматизацию приемочных тестов, а также на детализированную документацию и отчеты по проекту, построенные на базе этих тестов. Он работает вместе с JUnit и BDD инструментами, такими как JBehave and Cucumber-JVM, и предоставляет обширный API для автоматизированного тестирования в тесной интеграции с Selenium Webdriver.

Thucydides разработан для решения следующих задач:
* Написание более гибких тестов, которые легче поддерживать
* Получение иллюстрированных, исчерипывающих (story-based) отчетов
* Ясная привязка тестов к требованиям
* Измерение покрытия требований

#### Thucydides workflow
**Шаг 1: Определение требований и приемочных критериев**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/story-cards.jpg">
                </p>
            </td>
            <td valign="top" align="left">
                <p>Thucydides начинается с <strong>требований</strong>, которые нужно реализовать. Для каждого требования есть <strong>приемочные критерии</strong>, которые лучше разъясняют требование. Приемочные критерии и автоматизирует фукидид.</p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 2: Моделирование требований**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/requirements.png">
                </p>
            </td>
            <td valign="top" align="left">
                <p>С помощью фукидида вы строите **простую модель** ваших требований на языке Java. Есть несколько способов моделирования требований, включая обычный Java класс, используя конвенцию структуры директорий или интегрируясь с сторонними инструментами, вроде Jira. Такой подход позволяет разработчику явным образом указать, какое требование тестирует каждый из тестов, а фукидиду - **отслеживать тестируемые фичи и требования**.</p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 3: Автоматизация приемочного тестирования**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/given-when-then.jpg">
                </p>
            </td>
            <td valign="top" align="left">
                <p> Далее описываются приемочные критерии языком бизнесс-домена, а автоматизаторы имплементируют их с помощью BBD, таких как **JBehave или Cucumber-JVM**, или с помощью **Java и JUnit**, так чтобы фукидид мог их запускать, но со статусом "**pending**" (тело теста не реализовано).</p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 4: Имплементация тестов**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/small-keyboard.png">
                </p>
            </td>
            <td valign="top" align="left">
                <p> Автоматизаторы теперь могут **имплементировать приемочные критерии** в форме тестов для реального AUT. Тесты можно делить на **степы** для лучшей **читабельности** и **легкой поддержки**. Для тестирования веб приложений используется **Selenium Webdriver**.</p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 5: Отчет о результатах теста**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/reports.jpg">
                </p>
            </td>
            <td valign="top" align="left">
                <p> Thucydides позволяет строить **детализированные отчеты** о результатах запуска тестов, включая:
                    <ul>
                        <li>**Историю** для каждого теста</li>
                        <li>**Скриншот** для каждого степа в тесте</li>
                        <li>**Результат** выполнения теста, включая **время** и **сообщения об ошибках**</li>
                </ul>
                </p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 6: Отчет о покрытии требований**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/coverage.jpg">
                </p>
            </td>
            <td valign="top" align="left">
                <p> Кроме отчетов о выполнении тестов фукидид также предоставляет информацию о:
                    <ul>
                        <li>Количестве **протестированных** требований</li>
                        <li>Количестве **выполненных** требований</li>
                        <li>количестве **требований, которые предстоит выполнить**</li>
                    </ul>
                </p>
            </td>
        </tr>
    </tbody>
</table>
**Шаг 7: Отчет о прогрессе на проекте**
<table width="100%" cellspacing="" cellpadding="5" border="0">
    <tbody>
        <tr>
            <td valign="top" align="left" style="padding-top: 13px;">
                <p>
                    <img width="250" border="0" alt="" src="/resources/history.jpg">
                </p>
            </td>
            <td valign="top" align="left">
                <p> Фукидид также предоставляет информацию по истории и прогрессе проекта:
                    <ul>
                        <li>Изменение количества разработанных фич во времени</li>
                        <li>Изменение количества имплементированных и протестированных фич во времени</li>
                        <li>Изменение количества упавших тестов во времени</li>
                    </ul>
                </p>
            </td>
        </tr>
    </tbody>
</table>
Как видно Thucydides - довольно сложный инструмент, простроенный вокруг концепции BDD и приемочного тестирования, использующий Webdriver для тестирования веб приложений.

Maven зависимость:
```
<dependency>
	<groupId>net.thucydides</groupId>
	<artifactId>thucydides</artifactId>
	<version>0.9.273</version>
</dependency>

```


Более детальную информацию о нем вы найдете на официальном сайте - http://thucydides.info/

