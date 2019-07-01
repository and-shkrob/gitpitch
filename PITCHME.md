---?image=template/img/slide-1.jpg
@title[Sherlock Web Services]

@snap[north slide1]
<h1>Sherlock</h1>
@size[80%](Web Services Overview)
@snapend

@snap[east author-box]
@fa[github](https://github.com/and-shkrob/gitpitch) <br/>
@snapend

---
@snap[north-west]
<h4>@color[#1C60AC](Web Services)</h4>
@snapend

@snap[west list-content-concise span-100]
@ol[](false)
- **Driverportal** - @css[text-gray](driver portal, preallocation)
- **Driver-ws** - @css[text-gray](driver app)
- **External-ws** - @css[text-gray](telephony, misc)
- **Adler** - @css[text-gray](third-party bookings)
- **Sherbook** - @css[text-gray](booking portal, smartphones)
- **AMP** - @css[text-gray](account management portal)
- **DAB** - @css[text-gray](dashboards)
@olend
<br><br>
@snapend

@snap[south-west]
@size[80%](Все веб-сервисы работают на основе HTTP протокола)
@snapend

---
<h4>@color[#1C60AC](HTTP Cookie)</h4>

```bash
#Request
GET http://demo-stable.sherlock.com/sherbook/portal/serverTime?1561894276240

#Response
Set-Cookie:  JSESSIONID=7611617F64FC98BC2163788424C0A54E; Path=/sherbook; HttpOnly

#Next request
Cookie: JSESSIONID=7611617F64FC98BC2163788424C0A54E
```

@[1-5](Получив HTTP-запрос, вместе с откликом сервер может отправить заголовок  Set-Cookie)
@[5](Директива Path определяет область видимости cookie)
@[5](Куки HTTPonly не доступны из JavaScript)
@[7-8](Cookie обычно запоминаются браузером и посылаются в значении заголовка Cookie с каждым новым запросом к одному и тому же серверу)
@[7-8](Http не имеет состояния, но имеет сессию. Использование HTTP cookie позволяет связать запрос с состоянием на сервере)

---

<h4>@color[#1C60AC](HTTP Cache)</h4>

```bash
GET http://demo-stable.sherlock.com/portal/js/messages-57ae54605c890.js
#Response
Last-Modified: Sun, 30 Jun 2019 01:35:21 GMT
Date: Sun, 30 Jun 2019 12:29:14 GMT
ETag: W/"1216099-1561858521000"

#Request
GET http://demo-stable.sherlock.com/portal/js/messages-57ae54605c890.js
If-Modified-Since: Sun, 30 Jun 2019 01:35:21 GMT
If-None-Match: W/"1216099-1561858521000"

#Not modified
GET http://demo-stable.sherlock.com/portal/js/messages-57ae54605c890.js
304 Not Modified

#Cache control
Cache-control: no-store no-cache max-age=31536000 must-revalidate
Expires: Sun, 30 Jun 2019 01:35:21 GMT

```

@[1-5](Сервер в заголовках задает директивы для клиента о времени устаревания ответа)
@[7-10](Для валидации кешированния клиент посылает заголовки if-modified-since и if-none-match, полученный в ответе от сервера в прошлый раз)
@[12-14](Если валидация успешна, то сервер не посылает ответ, а возвращает статус 304. Данные возвращаются из кеша)
@[16-18](Для управления кешированием можно задавать директивы через заголовки Cache-control и Expires)

---

<h4>@color[#1C60AC](HTTP Cache)</h4>

@snap[midpoint span-100]
![Cache](template/img/cached.png)
@snapend

---

<h4>@color[#1C60AC](HTTP Authentication)</h4>

```bash
#Initial Request
GET https://app.mostaxi.ru/app/

401 Unauthorized
WWW-Authenticate: Basic realm="Unauthorized"

#Invalid request
GET https://app.mostaxi.ru/app/
Authorization: Basic MTIzOjEyMw==

401 Unauthorized
WWW-Authenticate: Basic realm="Unauthorized"

#Correct request
GET https://app.mostaxi.ru/app/
Authorization: Basic aGF1bG1vbnQ6SGF1IW0wbnQ=

200 OK
```

@[1-6](При попытке доступа к защищенным ресурсам, сервер возвращает 401)
@[1-6](Тип аутентификации возвращается в заголовке WWW-Authenticate)
@[7-13](Данные передаются в Authorization заголовке)
@[14-18](Верные данные обрабатываются и возвращается статус 200)

@ul[para]
- Подобным образом работают и другие типы авторизаций. Sherlock использует Basic и Bearer схемы
@ulend

---

<h4>@color[#1C60AC](HTTP Session vs Token)</h4>

@ul[para]
- [[TX-19264]](https://youtrack.haulmont.com/issue/TX-19264) Sherbook. Get rid of session_id cookie @css[text-green]([Verified])
- [[TX-19298]](https://youtrack.haulmont.com/issue/TX-19298) Quickbooker: get rid of session_id cookie @css[text-green]([Verified])
- [[TX-20346]](https://youtrack.haulmont.com/issue/TX-20346) Cross-application credentials @css[text-green]([Fixed])
- [[TX-20258]](https://youtrack.haulmont.com/issue/TX-20258) Driver-WS V2 get rid of using HTTP-session @css[text-blue]([Open, 47])
- [[TX-19155]](https://youtrack.haulmont.com/issue/TX-19155) Webportal: ability to login under 2 or more logins @css[text-blue]([Open, 47])
- [[TX-20624]](https://youtrack.haulmont.com/issue/TX-20624) Полностью уйти от использования jsessionid, http session, cookies в sherbook @css[text-blue]([Epic])
@ulend

---

<h4>@color[#1C60AC](RESTful API)</h4>

@ul[]
* Работает на основе HTTP
* Не имеет состояния
* Использует application/json и application/xml тип контента для обмена сообщениями
* Структура API формируется из директорий и ресурсов
* GET - получение ресурса
* POST - создание ресурса / операция над ресурсом
* PUT - изменение ресурса
* DELETE - удаление ресурса
@ulend

---

<h4>@color[#1C60AC](Swagger UI)</h4>

@ul[](false)
* Интерфейс для отображения документации и структуры API в удобночитаемом виде
* Позволяет запускать запросы, авторизовываться
* Предоставляет примеры запросов для быстрого формирования Json
@ulend

UI доступен для следующих веб-сервисов:

@ul[para](false)
- [Sherbook](http://demo-stable.sherlock.com/sherbook/swagger-ui.html#/) /sherbook/swagger-ui.html
- [AMP](http://demo-stable.sherlock.com/external-ws/amp/swagger-ui.html#/) /external-ws/amp/swagger-ui.html
- [Dab](http://demo-stable.sherlock.com/external-ws/dab/swagger-ui.html#/) /external-ws/dab/swagger-ui.html
- [External-ws](http://demo-stable.sherlock.com/external-ws/swagger-ui.html#/) /external-ws/swagger-ui.html
@ulend

---

<h4>@color[#1C60AC](Swagger UI)</h4>

@snap[midpoint span-100]
![Swagger UI](template/img/swagger.png)
@snapend

---

<h4>@color[#1C60AC](HTTP Request)</h4>

```bash
#HTTP Request
"URL"
"Method"
"Query parameters"
"Path variables"
"Headers"
"Cookies"
"Form-data"
"Body"
```
<br>
```java
    given(sherbook())
            .get("status")
            .then()
            .statusCode(200)
            .body(is("OK"))
```
@[2, 10](Я хочу сделать запрос в шербук)
@[3, 11](Хочу получить статус веб-сервиса)
@[12](Запустить запрос и затем ...)
@[13](Проверить, что запрос успешно вернул статус 200)
@[14](Проверить, что в ответе пришло слово OK)
@[10-14](Итоговый результат)

---?image=template/img/slide-1.jpg
@title[Sherlock Integration Tests]

@snap[north slide1]
<h1>Sherlock</h1>
@size[80%](Integration Tests)
@snapend

---

<h4>@color[#1C60AC](API Generator)</h4>

@ul[text-6](false)
* Swagger публикует структуру api в виде Json по спецификации [OpenAPI](https://swagger.io/specification/)
* Документацию можно найти по относительному пути **/v2/api-docs**, например http://demo-stable.sherlock.com/external-ws/amp/v2/api-docs
* Генератор парсит json доки. Из полученных данных генерируется модель данных и классы api
* Для генерации используется язык шаблонов Freemarker (.ftl)
@ulend

Шаблоны:
@ul[text-6](false)
* controller.ftl - API
* enumClass.ftl - модель enumeration
* groovyClass.ftl - модель dto
* macro.ftl - freemarker макросы
* reserved_words.ftl - список зарезервированных слов groovy/java
@ulend

---

<h4>@color[#1C60AC](API Generator)</h4>

```json
{
  "baseUrl": "http://testdev-stable.sherlock.com/",
  "apiDocsPath": "v2/api-docs",
  "apis": [
    {
      "name": "sherbook",
      "path": "sherbook"
    },
    {
      "name": "external-ws",
      "path": "external-ws"
    },
    {
      "name": "am-ws",
      "path": "external-ws/amp"
    },
    {
      "name": "dashboards",
      "path": "external-ws/dab"
    }
  ]
}
```
@[2](Адрес сервера)
@[3](Путь к swagger-докам)
@[4-21](Список веб-сервисов)

---

<h4>@color[#1C60AC](API Generator)</h4>

Запустить генератор можно через IDE *@color[#1C60AC](com.haulmont.taxi.tool.ws.generation.Generator)*
или из gradle *@color[#1C60AC](gw :ws:executeGenerator)*

Все изменения в модели или api видны в version control вкладке

![Changes](template/img/changes.png)

Любой класс можно защитить от изменений, если повесить на него @color[#1C60AC](@OnRegenerate) аннотацию со значением DENY

---

<h4>@color[#1C60AC](API Generator)</h4>

![Changes](template/img/changes2.png)

---

<h4>@color[#1C60AC](API Generator)</h4>

Структура сгенерированных файлов

```text
└── ws
    └── com.haulmont.taxi.tool.ws.api
        ├── am_ws
        │   ├── api
        │   │   ├── AccountControllerApi
        │   │   └── ...
        │   ├── model
        │   │   ├── enums
        │   │   │   ├── Level
        │   │   │   └── ...
        │   │   ├── AccountDto
        │   │   └── ...
        │   └── readme.md
        ...
```

---

<h4>@color[#1C60AC](API Generator)</h4>

Описание каждого метода доступно как javadocs. По нажатию ctrl + Q можно включить preview mode

![Javadocs](template/img/javadocs.png)

---

<h4>@color[#1C60AC](API Usage)</h4>

```groovy
    @Test
    void testLoginRetail() {
        LoginControllerApi.loginRetailUsingPOST().execute { 
            phone = '+79171671649'; password = '624249'
        }.then().statusCode(200)
       
        def retailResponse = LoginControllerApi.loginRetailUsingPOST().extract { 
            phone = '+79171671649'; password = '624249'
        }
        assertTrue retailResponse.sessionToken as boolean
        
        List<AccountAddressDto> addresses = AccountAddressesControllerApi.getAccountAddressesUsingGET()
                .limit(100)
                .offset(0)
                .extract()
    }
```
@[3-5](Чтобы запустить реквест достаточно выбрать нужное api, метод и выполнить execute)
@[3-5](Execute может принимать body или bodyClosure)
@[3-5](Ответом является стандартный RestAssured Response объект)
@[7-10](Если метод имеет response body, то можно выполнить extract. Метод возвращает типизированный ответ)
@[12-15](Если метод имеет параматеры запроса, path variables и прочие настраиваемы параметры, то к ним можно образатся как к методам билдера)

---

<h4>@color[#1C60AC](API Authorization)</h4>

@color[#1C60AC](@Authorized) аннотация позволяет авторизовать RestAssured тест. Поле id данной аннотации ссылается на значение из конфигурации

```json
{
  "source": {
    "external-ws/amp": {
      "defaultAuthorizationPath": "/api/user/login",
      "authorizations": {
        "default": {
          "path": "",
          "body": {
            "username": "web_service_test",
            "password": "12345",
            "locale": "en_GB"
          }
        }
      }
    }
  }
}
```
@[2](source: Ключ - относительный путь веб-сервиса, Значение - конфиг авторизаций)
@[3](Относительный путь веб-сервиса)
@[4](URL, который используется по-умолчанию для запроса авторизации)
@[5](Список авторизаций)
@[6](ID авторизации. Это значение используется в коде)
@[7](Переопределение url)
@[8-12](Тело запроса на авторизацию)

---

<h4>@color[#1C60AC](API Authorization)</h4>

@ul[text-6](false)
* @color[#1C60AC](AuthorizationListener) - lookup class-level, method-level annotations, register authorizations in thread local filter
* @color[#1C60AC](ThreadLocalAuthFilter) - remember thread-local auth id, authorize before first request, tune headers
@ulend

---

<h4>@color[#1C60AC](API Authorization Code Example)</h4>

```java
@Authorized(id = "default")
public class MyTest {

    @Test
    @Authorized(false)
    void unauthoruzedTest() { }
    
    @Test
    void authorizedTest() {}
    
    @Test
    @Authorized(id = "not_default")
    void unauthoruzedTest() { }
    
}
```
@[1-2](Можно использовать аннотацию на уровне класса, это применит авторизацию ко всем тестам данного класса)
@[4-6](Можно отключить авторизацию на конкретном тесте)
@[1-2, 8-9](Данный тест будет авторизован по-умолчанию)
@[11-13](Данный тест переопределяет авторизацию по-умолчанию)

---

<h4>@color[#1C60AC](Allure)</h4>

@ul[para](false)
- @css[text-8](Основная задача - генерация удобночитаемых отчетов)
@ulend

@snap[west span-80]
@ul[text-6](false)
* @color[#1C60AC](Overview) - entry point for every report. Dashboards and widgets
* @color[#1C60AC](Categories) - defects Classification
* @color[#1C60AC](Suites) - standard structural representation of executed tests
* @color[#1C60AC](Graphs) - different statistics collected from the test data
* @color[#1C60AC](Timeline) - visualizes retrospective of tests execution
* @color[#1C60AC](Behaviors) - groups test results according to Epic, Feature and Story tags
* @color[#1C60AC](Packages) - packages tab represents a tree-like layout of test results, grouped by different packages
@ulend
@snapend

@snap[east span-20 max-height]
![Allure Sidebar](template/img/allure_sidebar.png)
@snapend

---

<h4>@color[#1C60AC](Allure Report)</h4>

![Allure Tab](template/img/allure_tab.png)

---

<h4>@color[#1C60AC](Allure Report)</h4>

![Allure ra](template/img/allure_passed_ra_test.png)

---

<h4>@color[#1C60AC](Allure Report)</h4>

![Allure ui](template/img/allure_failed_ui_test.png)

---

<h4>@color[#1C60AC](Allure Code)</h4>

```java
@Step
void someStep(){}

@Step("Проверка разности числа {num1} и числа {num2}")
void checkSubtractionStep(int num1, int num2, int expectedResult) {}

@Attachment(value = "Вложение", type = "application/json", fileExtension = ".txt")
byte[] getBytes(String resourceName)  {}

@Description(value "Описание теста")

//Аннотации функциональности
@Epic("Allure examples")
@Feature("Junit 4 support")
public class MyTest {

    @Test
    @Story("Base support for bdd annotations")
    @Story("Advanced support for bdd annotations")
    public void testSomething() throws Exception {
        // ...
    }
}
```
@[1-2](Step аннотация позволяет добавить новый шаг в отчет)
@[4-5](Step умеет обращаться к параметрам метода)
@[7-8](Attachment аннотация позволяет добавить собственное вложение в тест)
@[10](Description аннотация добавляет описание в тест)
@[12-25](Epic, Feature, Story служат для группировки тестов по функциональным особенностям)

---

<h4>@color[#1C60AC](Allure Code)</h4>

```java
//Аннотации ссылок

@Issue("TX-21152")
@TmsLink("TX-21152")
@Link("https://youtrack.haulmont.com/issue/TX-21152")

@Severity(SeverityLevel.MINOR)
```
@[1-5](Issue - ссылка на багтрекинг тикет, TmsLink - ссылка на тикет с описание тестов, Link - произвольная ссылка)
@[6-7](Severity - уровень важности)

<br>

Gradle tasks:
@ul[text-6](false)
* @color[#1C60AC](allureReport) - сгенерировать отчет. Данные берутся из директории allure-results 
* @color[#1C60AC](allureServe) - сгенерировать и отобразить отчет
@ulend
