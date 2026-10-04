# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) | Кириленко Олександра Вікторівна|
| Група |ІПЗ 2.01 |
| Номер варіанта | 10|
| Індивідуальний домен | example.com |
| «Чужий» домен для завдання A.3.1 (варіант ± 20) | unicode.org|
| Середовище виконання | macOS|
| Дата виконання |04.10.2026 |

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
nc -C example.com 80
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: example.com
Connection: close
```

**Відповідь:**

```
Last login: Sun Oct  4 00:18:42 on ttys001
macbook@MacBook-Air-MacBook ~ % nc -C example.com 80
GET / HTTP/1.1
Host: example.com
Connection: close

HTTP/1.1 200 OK
Date: Sat, 03 Oct 2026 21:24:38 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: close
Server: cloudflare
Last-Modified: Fri, 02 Oct 2026 16:11:13 GMT
Allow: GET, HEAD
Accept-Ranges: bytes
Age: 4370
cf-cache-status: HIT
CF-RAY: a44f03c53de7ca4d-KBP
alt-svc: h3=":443"; ma=86400

241
<!doctype html><html lang=en><head><meta charset=utf-8><link rel=icon href=data:,><meta name=viewport content="width=device-width,initial-scale=1"><title>Example Domain</title><style>html{color-scheme:light dark;background:light-dark(#eee,#222)}body{font:16px/1.6 system-ui,sans-serif;max-width:26em;margin:auto;padding:25vh 2em 2em;text-align:center}</style></head><body><p>This domain is for use in documentation examples without needing permission. This is not a service; avoid relying on it for testing and monitoring purposes.</p><script src=/s.js></script></body></html>

0


```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
Last login: Sun Oct  4 00:24:27 on ttys001
macbook@MacBook-Air-MacBook ~ % 
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc example.com 80
HTTP/1.1 400 Bad Request
Server: cloudflare
Date: Sat, 03 Oct 2026 21:27:02 GMT
Content-Type: text/html
Content-Length: 155
Connection: close
CF-RAY: -

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: unicode.org\r\nConnection: close\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
HTTP/1.1 200 OK
Date: Sun, 04 Oct 2026 18:44:00 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Server: cloudflare
Content-Security-Policy: upgrade-insecure-requests;
Last-Modified: Thu, 02 Mar 2023 00:38:51 GMT
Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=J%2BbR%2F98x%2FPWx%2FynsRnRJr5GcydzylVZFW0bE6MWZCtxNXfzxlZqvQl8EtvwQnvQNLUbV5KmyYxm6D930R2SljMm94GlE7o9tgz1Ii5bzrXsAymC%2BCpMi6Aj5ifZ3"}]}
Vary: Accept-Encoding
Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
cf-cache-status: DYNAMIC
CF-RAY: a45655db6f2fd3b9-FRA

db
<html><head>
<meta http-equiv="refresh" content="0; url=http://home.unicode.org/">
<title>Index</title>
</head>
<body>
Automatic redirect: <a href="http://home.unicode.org/">http://home.unicode.org/</a>
</body></html>


0

```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
HTTP/1.1 409 Conflict
Date: Sun, 04 Oct 2026 18:46:13 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 16
Connection: close
X-Frame-Options: SAMEORIGIN
Referrer-Policy: same-origin
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Server: cloudflare
CF-RAY: a456591d182c78c0-FRA
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
printf 'GET / HTTP/1.0\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
HTTP/1.1 403 Forbidden
Date: Sun, 04 Oct 2026 18:53:19 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a45663866b24d242-FRA

error code: 1003
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: example.com\r\n\r\nGET / HTTP/1.1\r\nHost:example.com\r\nConnection: close\r\n\r\n' | nc -C example.com 80
```

**Вивід:**

```
HTTP/1.1 404 Not Found
Date: Sun, 04 Oct 2026 18:53:56 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Server: cloudflare
Age: 5774
cf-cache-status: HIT
CF-RAY: a456646dc9aed259-FRA
alt-svc: h3=":443"; ma=86400

241
<!doctype html><html lang=en><head><meta charset=utf-8><link rel=icon href=data:,><meta name=viewport content="width=device-width,initial-scale=1"><title>Example Domain</title><style>html{color-scheme:light dark;background:light-dark(#eee,#222)}body{font:16px/1.6 system-ui,sans-serif;max-width:26em;margin:auto;padding:25vh 2em 2em;text-align:center}</style></head><body><p>This domain is for use in documentation examples without needing permission. This is not a service; avoid relying on it for testing and monitoring purposes.</p><script src=/s.js></script></body></html>

0

HTTP/1.1 200 OK
Date: Sun, 04 Oct 2026 18:53:56 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: close
Server: cloudflare
Last-Modified: Fri, 02 Oct 2026 16:11:02 GMT
Allow: GET, HEAD
Accept-Ranges: bytes
Age: 2
cf-cache-status: HIT
CF-RAY: a456646de9ecd259-FRA
alt-svc: h3=":443"; ma=86400

241
<!doctype html><html lang=en><head><meta charset=utf-8><link rel=icon href=data:,><meta name=viewport content="width=device-width,initial-scale=1"><title>Example Domain</title><style>html{color-scheme:light dark;background:light-dark(#eee,#222)}body{font:16px/1.6 system-ui,sans-serif;max-width:26em;margin:auto;padding:25vh 2em 2em;text-align:center}</style></head><body><p>This domain is for use in documentation examples without needing permission. This is not a service; avoid relying on it for testing and monitoring purposes.</p><script src=/s.js></script></body></html>

0
```

**Кількість отриманих відповідей: 2**

**Коди стану отриманих відповідей:404 та 200**

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl -v --http1.1 http://example.com/ -o /dev/null
```

**Вивід:**

```
Last login: Sun Oct  4 01:43:33 on ttys001
macbook@MacBook-Air-MacBook ~ % curl -v --http1.1 http://example.com/ -o /dev/null
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host example.com:80 was resolved.
* IPv6: (none)
* IPv4: 104.20.23.154, 172.66.147.243
*   Trying 104.20.23.154:80...
* Connected to example.com (104.20.23.154) port 80
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/8.7.1
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Sat, 03 Oct 2026 22:47:26 GMT
< Content-Type: text/html; charset=utf-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Server: cloudflare
< Last-Modified: Fri, 02 Oct 2026 16:11:13 GMT
< Allow: GET, HEAD
< Accept-Ranges: bytes
< Age: 9338
< cf-cache-status: HIT
< CF-RAY: a44f7d146a3eca4d-KBP
< alt-svc: h3=":443"; ma=86400
< 
{ [589 bytes data]
100   577    0   577    0     0   7720      0 --:--:-- --:--:-- --:--:--  7797
* Connection #0 to host example.com left intact
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** <`iana.org`>

**Підстава для використання резервного ресурсу (заповнюють за потреби):у завданні A.1 отримано код стану, відмінний від класу 3xx,а саме 2xx**

**Команда:**

```
openssl s_client -connect iana.org:443 -servername iana.org -crlf -quiet
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: iana.org
Connection: close
```

**Вивід:**

```
Last login: Sun Oct  4 01:52:51 on ttys001
macbook@MacBook-Air-MacBook ~ % 
openssl s_client -connect iana.org:443 -servername iana.org -crlf -quiet
depth=3 C = US, ST = New Jersey, L = Jersey City, O = The USERTRUST Network, CN = USERTrust RSA Certification Authority
verify return:1
depth=2 C = GB, O = Sectigo Limited, CN = Sectigo Public Server Authentication Root R46
verify return:1
depth=1 C = GB, O = Sectigo Limited, CN = Sectigo Public Server Authentication CA OV R36
verify return:1
depth=0 C = US, ST = California, O = Internet Corporation For Assigned Names and Numbers, CN = *.iana.org
verify return:1
GET / HTTP/1.1
Host: iana.org
Connection: close

HTTP/1.1 301 Moved Permanently
Date: Sat, 03 Oct 2026 22:58:58 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Wed, 07 Oct 2026 22:58:58 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
read:errno=0
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:**

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 |HTTP/1.1  |200 OK|Повідомляє, що HTTP-запит успішно оброблено сервером |Сервер |Відповідь із кодом стану 200 OK показує,що запит клієнта успішно оброблений|
| 2 | Date| Sat, 03 Oct 2026 21:24:38 GMT|Показує точний час і дату, коли сервер сформував цю відповідь | сервер|Сервер автоматично генерує цей заголовок в момент відправки, щоб клієнт знав дійсний час |
| 3 |Content-Type | text/html; charset=utf-8| Описує формат переданих даних(у нас це HTML-код) та кодування символів|сервер | Вказується для того щоб браузер зрозумів, як саме відображати отриманий текст|
| 4 | Transfer-Encoding|chunked | Вказує, що тіло відповіді передається частинами(розбитий на фрагменти), а не цілим файлом одразу|сервер | Використовується коли повний розмір сторінки заздалегідь невідомий|
| 5 |Connection | close| Означає що після завершення передачі даних з'єднання буде закрито| сервер| Ми самі написали Connection: close у запиті, і сервер підтверджує це у відповіді закриваючи|
| 6 | Server| cloudflare| Означає яке програмне забезпечення чи сервіс обробили запит| проміжний вузол|виступає як проксі |
| 7 | Last-Modified| Fri, 02 Oct 2026 16:11:13 GMT|Дата і час останньої зміни цього ресурсу на сервері | сервер| |
| 8 | Allow| GET, HEAD|Перераховує HTTP-методи які підтримує цей ресурс | сервер| Допомагає зрозуміти які запити сюди взагалі можна надсилати(наприклад: GET або HEAD працюють, а POST чи PUT ні)|
| 9 |Accept-Ranges | bytes| Показує чи підтримує сервер запити на завантаження файлу частинами|сервер | Значення bytes означає, що можна завантажувати файл частинами|
| 10 |Age | 4370| Час у секундах |Не визначено | Недостатньо знань|
| 11 |cf-cache-status |HIT| | Не визначено|  Недостатньо знань|
| 12|CF-RAY |a44f03c53de7ca4d-KBP| |Не визначено| Недостатньо знань |
| 13 |alt-svc | h3=":443"; ma=86400| |Не визначено| Недостатньо знань|


---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

Несподіваним виявилося те, що після введення команди printf 'GET / HTTP/1.0\r\n\r\n' | nc example.com 80 у кінці виводу з’явився рядок error code: 1003 

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

Найбільше утруднення викликало при визначенні походження цих рядків: Age: 4370
cf-cache-status: HIT
CF-RAY: a44f03c53de7ca4d-KBP
alt-svc: h3=":443"; ma=86400
оскільки вони не є стандартними і я ще жодного разу їх не зустрічала

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

У завданні А.3.1 у виводі не було зазначено обсяг тіла відповіді у стандартному вигляді Content-Length. Як у такому випадку дізнатися розмір? 
У завданні А.1 є такий HTTP-заголовок: Server: cloudflare.Якщо Server - це назва заголовка, а Cloudflare у цьому випадку виступає як проміжний вузол, то що правильно вказати в таблиці в графі «Походження»: сервер чи проміжний вузол?

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

Сервер чекав порожній рядок, тому що  HTTP/1.1 порожній рядок є обов’язковим роздільник між заголовками запиту та його тілом і показує серверу, що заголовки запиту завершені(nc самостійно не додає цей роздільник, тому сервер не починав обробку запиту, доки користувач не ввів додатковий порожній рядок) 

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

В A.1 та A.3.1 видно, що сервер успішно обробляє запит, коли в полі Host вказано правильне доменне ім’я. Наприклад: у A.1 при Host: example.com сервер повернув HTTP/1.1 200 OK, а в A.3.1 при Host: unicode.org також отримано HTTP/1.1 200 OK

В A.2, де використовується HTTP/1.1 без поля Host, сервер надсилає HTTP/1.1 400 Bad Request, оскільки для HTTP/1.1 цей заголовок є обов’язковим. 
В A.3.2 з неіснуючим доменом opism-pr02.invalid отримуємо HTTP/1.1 409 Conflict. В A.3.3 при використанні HTTP/1.0 без Host сервер надсилає HTTP/1.1 403 Forbidden
Отже, поле Host допомагає серверу визначити, до якого саме сайту звертається клієнт, особливо коли на одній IP-адресі розміщено декілька сайтів

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

У завданні A.4 надійшло дві відповіді: перша з кодом 404 Not Found, а друга - 200 OK.
Так, відповідь сервера на порту 80 залежить від запитаного шляху.Для /opism-pr02-12345 він надсилає 404 Not Found, бо такого шляху немає, а для / — 200 OK тому що сторінка існує. 
Це показує, що сервер приймає HTTP-запити та надсилає різну відповідь залежно від того, що саме ми запитали.
Дві відповіді в одному з’єднанні означають, що клієнт може робити кілька запитів без створення нового з’єднання

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

Програма curl самостійно додала поля User-Agent та Accept. User-Agent показує серверу, що запит виконує програма curl, а Accept, що клієнт може прийняти будь-який тип даних. 
Ці поля не є обов’язковими, але вони дають серверу додаткову інформацію про клієнта

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

У виводах A.1 та A.5 є ознака проміжного вузла - поле Server: cloudflare.Воно показує, що запит обробляється сервісом Cloudflare, а у  A.6 поле Server: Apache вказує на ПЗ сервера, тобто проміжний вузол не виявлено 

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | cf-cache-status: HIT|А.1 |
| 2 | CF-RAY: -| А.2|
| 3 | Cache-Control: max-age=345600| А.6|

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):**

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) |example.com | 1.1 |200 | 241| — |
| A.2 | поле відсутнє | 1.1 | 400| 155|ні|
| A.3.1 | unicode.org| 1.1 | 200| | ні|
| A.3.2 | `opism-pr02.invalid` | 1.1 | 409| 16|ні |
| A.3.3 | поле відсутнє | 1.0 | 403| 17| ні|

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

У кожній пробі ми по-різному змінювали запит: прибирали Host, змінювали його значення або використовували іншу версію HTTP. 
Сервер на це реагував по-різному: коли Host був правильний - повертав 200 OK, а при неправильному або відсутньому Host повертав помилки 400, 409 або 403. Це показує, що для сервера важливо, яке саме ім’я вказане в Host і в якому форматі зроблений запит

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** <так>


| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | Gemini 3.5 Flash-Lite| Частина В| Чи сервер cloudflare може виступати як проксі ? | Записано в обґрунтуванні: «виступає як проксі»|
| 2 | Gemini 3.5 Flash-Lite| Частина А| Що означає код стану 409? | Дізналася для себе точніше, що це означає, і записала в конспект|

**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.

---

*ОПІСМ (ОК-13) · Практична робота № 2 · бланк звіту*
