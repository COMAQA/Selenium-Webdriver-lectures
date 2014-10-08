# Репортинг
Репортинг - построение читаемого отчета о результатах выполнения тестов. В отчете прежде всего должна присутствовать информация о:
* Количестве запущенных тестов
* По каждому тесту:
 * его название
 * название сьюта
 * результат
 * время выполнения
 * сообщение об ошибке, если тест упал
* Количество успешно выполненных тестов
* Количество упавших тестов
* Время выполнения всех тестов
* Дата и время запуска

Эту информацию должен содержать любой отчет о выполнении тестов. Также отчет может содержать дополнительную информацию, полезную для дебаггинга:
* Полный стек трейс, если тест упал
* Последовательность степов (test story) и их результат
* Встроенные скриншоты на каждый степ и/или при падении теста
* Встроенные логи и/или другие attachments (любые текстовы и медиа файлы, включая html dump страницы)
* Значения переменных при DDT
* Значения переменных тестового окружения (версия приложения, браузер и другое)

Кроме этого отчет может содержать отчеты о покрытии требований и другую визуальную информацию:
* Чарты и графики о результатах выполнения тестов
* Чарты и графики о покрытии требований
* Другая визуальная информация сводного характера

Информация о результатах теста получается и сохраняется после запуска тестов тест раннером. Часто это xml файл. Для тестовых фреймворков XUnit xml файл с результатами имеет один и тот же формат, что позволяет инструментам, строящим визуализированный отчет (чаще всего html), работать с ним единообразно.

Таким образом информация о результатах прохождения тестов собирается и сохраняется при запуске тестов, но визулизироваться может на следующих этапах:
* Сразу после запуска непосредственным тест раннером (например, testng или junit)
* При запуске билд инструментом (surefire plugin для maven)
* При запуске через CI (соотвествующие плагины)
* Отдельный инструмент для генерации отчета (вручную)

Кроме того, если набор информации, которую собирает тест раннер, вам кажется неполным, то можно использовать различные test report фреймворки, которые собирает больше информации и строят более красивые и полные отчеты либо создать такой инструмент самому. Как правило, такие инструменты состоят из 3 компонентов:
1. Адаптер для тест раннера (listener). Этот модуль подключается к тест раннеру (непосредственному или встроенному в билд инструмент) и собирает информацию о выполнении тестов независимо от встроенного в тест раннер механизма.
2. Модель - это информация о формате хранения собранной информации о выполнении тестов (структура xml файла, например)
3. Генератор html репорта на основе имеющейся модели по информации, сохраненной в конкретном xml файле.

Примером такого репорт фреймворка может служить ReportNG, который подключается и работает с TestNG тест раннером. Пример его конфигурации с maven:
```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.7</version>
            <configuration>
                <testSourceDirectory>${basedir}/src/main/java/</testSourceDirectory>
                <testClassesDirectory>${project.build.directory}/classes/</testClassesDirectory>
                <properties>
                    <property>
                        <name>usedefaultlisteners</name>
                        <value>false</value>
                    </property>
                    <property>
                        <name>listener</name>
                        <value>org.uncommons.reportng.HTMLReporter, org.uncommons.reportng.JUnitXMLReporter</value>
                    </property>
                </properties>
                <workingDirectory>${project.basedir}</workingDirectory>
                <argLine>-Dmaven.browser</argLine>
            </configuration>
        </plugin>
    </plugins>
</build>
```
Пример репорта можно увидеть по адресу http://reportng.uncommons.org/sample/index.html 
Официальный сайт фреймворка http://reportng.uncommons.org/

Минус этого фреймворка заключается в том, что он работает только с TestNG, но не работает с JUnit. Более универсальный фреймворк такого же типа - allure framework разработанный в yandex.
Allure framework умеет работать как с TestNG, так и с JUnit. Более того, он может работать с BDD инструментами (Cucumber) и с тест раннерами на других языках (NUnit - С#, pytest - python и другие). Такая универсальность делает его отличным выбором для любого проекта по автоматизации (пожалуй кроме случая с thucydides).

Подключается к maven allure следующим образом:
```
 <build>
    plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.16</version>
            <configuration>
                <testSourceDirectory>${basedir}/src/main/java/</testSourceDirectory>
                <testClassesDirectory>${project.build.directory}/classes/</testClassesDirectory>
                <testFailureIgnore>false</testFailureIgnore>
                <argLine>-javaagent:${settings.localRepository}/org/aspectj/aspectjweaver/${aspectj.version}/aspectjweaver-${aspectj.version}.jar
                </argLine>
                <properties>
                    <property>
                        <name>listener</name>
                        <value>ru.yandex.qatools.allure.testng.AllureTestListener</value>
                    </property>
                </properties>
                <argLine>-Dmaven.browser</argLine>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>org.aspectj</groupId>
                    <artifactId>aspectjweaver</artifactId>
                    <version>${aspectj.version}</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```
Также есть библиотека, предоставляющая дополнительный функционал, призванный сделать репорт еще более читаемым и полным.
```
<dependency>
    <groupId>ru.yandex.qatools.allure</groupId>
    <artifactId>allure-testng-adaptor</artifactId>
    <version>${allure.version}</version>
</dependency>
```
А также модуль для генерации html репорта:
```
<reporting>
        <excludeDefaults>true</excludeDefaults>
        <plugins>
            <plugin>
                <groupId>ru.yandex.qatools.allure</groupId>
                <artifactId>allure-maven-plugin</artifactId>
                <version>LATEST</version>
            </plugin>
        </plugins>
    </reporting>
```
При такой конфигурации maven проекта достаточно запустить тесты (<code>mvn clean test</code>), чтобы сгенерировался xml файл в отдельной папке с информацией, собранной allure-testNG адаптером. Команда <code>mvn site</code> сгенерирует html отчет в папке site. Пример отчета, а также полную документацию и примеры проектом вы найдете на сайте инструмента: http://allure.qatools.ru/



