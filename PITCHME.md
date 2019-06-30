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

@span[south-west]
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

---

<h4>@color[#1C60AC](HTTP Sessions)</h4>

```bash
```

---

<h4>@color[#1C60AC](HTTP Cache)</h4>

```bash
```

---

<h4>@color[#1C60AC](HTTP Authentication)</h4>

```bash
```