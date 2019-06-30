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
Web Services
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
304 GET http://demo-stable.sherlock.com/portal/js/messages-57ae54605c890.js

#Cache control
Cache-control: no-store no-cache max-age=31536000 must-revalidate
Expires: Sun, 30 Jun 2019 01:35:21 GMT

```

@[1-4](Сервер в заголовках задает директивы для клиента о времени устаревания ответа)
@[7-10](Для валидации кешированния клиент посылает заголовки if-modified-since и if-none-match, полученный в ответе от сервера в прошлый раз)
@[12-14](Если валидация успешна, то сервер не посылает ответ, а возвращает статус 304. Данные возвращаются из кеша)
@[16-18](Для управления кешированием можно задавать директивы через заголовки Cache-control и Expires)
@snap[south fragment span-100]
![Cache](template/img/cached.png)
@snapend

---

<h4>@color[#1C60AC](HTTP Authentication)</h4>

```bash
```