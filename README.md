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
<повний вивід>
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
<повний вивід>
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
printf 'GET / HTTP/1.0\r\n\r\n' | nc example.com 80
```

**Вивід:**

```
<повний вивід>
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
<повний вивід>
```

**Кількість отриманих відповідей:**

**Коди стану отриманих відповідей:**

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
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |
| 7 | | | | | |
| 8 | | | | | |

> Рядок наводять на кожне поле, яке реально надійшло. Зайві рядки вилучають, за потреби додають нові. Поле, походження якого встановити не вдалося, зазначають із позначкою «не визначено» та поясненням утруднення.

---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

<текст>

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

<текст>

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

<текст>

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

<відповідь>

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

<відповідь>

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

<відповідь>

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

<відповідь>

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

<відповідь>

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):**

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | | 1.1 | | | — |
| A.2 | поле відсутнє | 1.1 | | | |
| A.3.1 | | 1.1 | | | |
| A.3.2 | `opism-pr02.invalid` | 1.1 | | | |
| A.3.3 | поле відсутнє | 1.0 | | | |

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

<текст>

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** <так / ні>

Якщо так, заповнюють таблицю. Якщо ні, таблицю вилучають.

| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |

**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.

---

*ОПІСМ (ОК-13) · Практична робота № 2 · бланк звіту*
