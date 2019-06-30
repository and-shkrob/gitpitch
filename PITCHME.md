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


