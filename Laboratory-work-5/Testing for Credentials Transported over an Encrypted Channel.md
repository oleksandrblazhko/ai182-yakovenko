# Проверка учетных данных, передаваемых по зашифрованному каналу 

## Резюме 

Тестирование передачи учетных данных позволяет убедиться, что веб-приложения шифруют данные проверки подлинности при передаче. Это шифрование не позволяет злоумышленникам завладеть учетными записями путем прослушивания сетевого трафика . Веб-приложения используют HTTPS для шифрования информации при передаче как от клиента к серверу, так и от сервера к клиенту. Клиент может отправлять или получать данные аутентификации во время следующих взаимодействий: 

- Клиент отправляет учетные данные для запроса входа 
- Сервер отвечает на успешный вход токеном сеанса 
- Аутентифицированный клиент отправляет токен сеанса для запроса конфиденциальной информации с веб-сайта. 
- Клиент отправляет токен на веб-сайт, если он забыл свой пароль 

Неспособность зашифровать какие-либо из этих учетных данных при передаче может позволить злоумышленникам с инструментами анализа сети просмотреть учетные данные и, возможно, использовать их для кражи учетной записи пользователя. Злоумышленник может прослушивать трафик напрямую, используя Wireshark или аналогичные инструменты, или настроить прокси-сервер для захвата HTTP-запросов. Конфиденциальные данные должны быть зашифрованы при передаче, чтобы предотвратить это. 

Тот факт, что трафик зашифрован, не обязательно означает, что он полностью безопасен. Безопасность также зависит от используемого алгоритма шифрования и надежности ключей, используемых приложением. См. Тестирование слабой безопасности транспортного уровня , чтобы убедиться, что алгоритм шифрования достаточен. 

## Цели теста 

- Оцените, заставляет ли какой-либо вариант использования веб-сайта или приложения сервер или клиент обмениваться учетными данными без шифрования. 

## Как проверить 

Чтобы проверить транспорт учетных данных, зафиксируйте трафик между клиентом и сервером веб-приложений, которому требуются учетные данные. Проверьте учетные данные, переданные во время входа в систему и при использовании приложения с действительным сеансом. Чтобы настроить для теста: 


1. Настройте и запустите инструмент для захвата трафика, например один из следующих:
    * веб-браузера Инструменты разработчика 
    * Прокси, включая OWASP ZAP 

2. Отключите все функции или плагины, которые заставляют веб-браузер использовать HTTPS. Некоторые браузеры или расширения, такие как HTTPS Everywhere , будут бороться с принудительным просмотром , перенаправляя HTTP-запросы на HTTPS. 

В захваченном трафике ищите конфиденциальные данные, включая следующие: 

- Парольные фразы или пароли, обычно внутри тела сообщения 
- Токены, обычно внутри файлов cookie 
- Коды сброса учетной записи или пароля 

Для любого сообщения, содержащего эти конфиденциальные данные, убедитесь, что обмен произошел с использованием HTTPS (а не HTTP). В следующих примерах показаны захваченные данные, указывающие на пройденные или не пройденные тесты, когда веб-приложение находится на сервере с именем `www.example.org.`

## Авторизоваться 

Найдите адрес страницы входа и попытайтесь переключить протокол на HTTP. Например, URL-адрес для принудительного просмотра может выглядеть следующим образом: `http://www.example.org/login.`

Если страница входа обычно использует HTTPS, попробуйте удалить букву «S», чтобы увидеть, загружается ли страница входа как HTTP. 

Войдите в систему, используя действующую учетную запись, пытаясь принудительно использовать незашифрованный HTTP. При прохождении теста запрос на вход должен быть HTTPS: 

``` 
Request URL: https://www.example.org/j_acegi_security_check
Request method: POST
...
Response headers:
HTTP/1.1 302 Found
Server: nginx/1.19.2
Date: Tue, 29 Sep 2020 00:59:04 GMT
Transfer-Encoding: chunked
Connection: keep-alive
X-Content-Type-Options: nosniff
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: JSESSIONID.a7731d09=node01ai3by8hip0g71kh3ced41pmqf4.node0; Path=/; Secure; HttpOnly
ACEGI_SECURITY_HASHED_REMEMBER_ME_COOKIE=dXNlcmFiYzoxNjAyNTUwNzQ0NDU3OjFmNDlmYTZhOGI1YTZkYTYxNDIwYWVmNmM0OTI1OGFhODA3Y2ZmMjg4MDM3YjcwODdmN2I2NjMwOWIyMDU3NTc=; Path=/; Expires=Tue, 13-Oct-2020 00:59:04 GMT; Max-Age=1209600; Secure; HttpOnly
Location: https://www.example.org/
...
POST data:
j_username=userabc
j_password=My-Protected-Password-452
from=/
Submit=Sign in

```

- При входе в систему учетные данные шифруются из-за URL-адреса запроса HTTPS. 
- Если сервер возвращает информацию о куки-файле для токена сеанса, куки-файл также должен включать Secureво избежание раскрытия клиентом файла cookie по незашифрованным каналам позже. Ищите Secureключевое слово в заголовке ответа. 

Тест не пройден, если какой-либо логин передает учетные данные по HTTP, как показано ниже: 

``` 
Request URL: http://www.example.org/j_acegi_security_check
Request method: POST
...
POST data:
j_username=userabc
j_password=My-Protected-Password-452
from=/
Submit=Sign in
```
В этом неудачном тестовом примере: 

- URL-адрес получения `http://`и он раскрывает открытый текст j_usernameи j_passwordчерез почтовые данные. 
- В этом случае, поскольку тест уже показывает данные POST, раскрывающие все учетные данные, нет смысла проверять заголовки ответов (которые также, вероятно, раскрывают токен сеанса или файл cookie). 

## Создание аккаунта 

Чтобы проверить создание незашифрованной учетной записи, попробуйте принудительно перейти к HTTP-версии создания учетной записи и создать учетную запись, например: `http://www.example.org/securityRealm/createAccount`

Тест считается пройденным, если даже после принудительного просмотра клиент по-прежнему отправляет запрос новой учетной записи через HTTPS: 

```
Request URL: https://www.example.org/securityRealm/createAccount
Request method: POST
...
Response headers:
HTTP/1.1 200 OK
Server: nginx/1.19.2
Date: Tue, 29 Sep 2020 01:11:50 GMT
Content-Type: text/html;charset=utf-8
Content-Length: 3139
Connection: keep-alive
X-Content-Type-Options: nosniff
Set-Cookie: JSESSIONID.a7731d09=node011yew1ltrsh1x1k3m6g6b44tip8.node0; Path=/; Secure; HttpOnly
Expires: 0
Cache-Control: no-cache,no-store,must-revalidate
X-Hudson-Theme: default
Referrer-Policy: same-origin
Cross-Origin-Opener-Policy: same-origin
X-Hudson: 1.395
X-Jenkins: 2.257
X-Jenkins-Session: 4551da08
X-Hudson-CLI-Port: 50000
X-Jenkins-CLI-Port: 50000
X-Jenkins-CLI2-Port: 50000
X-Frame-Options: sameorigin
Content-Encoding: gzip
X-Instance-Identity: MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA3344ru7RK0jgdpKs3cfrBy2tteYI1laGpbP4fr5zOx2b/1OEvbVioU5UbtfIUHruD9N7jBG+KG4pcWfUiXdLp2skrBYsXBfiwUDA8Wam3wSbJWTmPfSRiIu4dsfIedj0bYX5zJSa6QPLxYolaKtBP4vEnP6lBFqW2vMuzaN6QGReAxM4NKWTijFtpxjchyLQ2o+K5mSEJQIWDIqhv1sKxdM9zkb6pW/rI1deJJMSih66les5kXgbH2fnO7Fz6di88jT1tAHoaXWkPM9X0EbklkHPT9b7RVXziOURXVIPUTU5u+LYGkNavEb+bdPmsD94elD/cf5ZqdGNoOAE5AYS0QIDAQAB
...
POST data:
username=user456
fullname=User 456
password1=My-Protected-Password-808
password2=My-Protected-Password-808
Submit=Create account
Jenkins-Crumb=58e6f084fd29ea4fe570c31f1d89436a0578ef4d282c1bbe03ffac0e8ad8efd6

```

- Подобно логину, большинство веб-приложений автоматически выдают токен сеанса при успешном создании учетной записи. если есть `Set-Cookie`:, убедитесь, что он имеет `Secure`;тоже атрибут.

Тест не пройден, если клиент отправляет запрос новой учетной записи с незашифрованным HTTP: 

```
Request URL: http://www.example.org/securityRealm/createAccount
Request method: POST
...
POST data:
username=user456
fullname=User 456
password1=My-Protected-Password-808
password2=My-Protected-Password-808
Submit=Create account
Jenkins-Crumb=8c96276321420cdbe032c6de141ef556cab03d91b25ba60be8fd3d034549cdd3
```

- Эта форма создания пользователя Jenkins раскрывала все новые данные пользователя (имя, полное имя и пароль) в данных POST на странице создания учетной записи HTTP. 

## Сброс пароля, изменение пароля или другие манипуляции с учетной записью 

Аналогично входу в систему и созданию учетной записи, если в веб-приложении есть функции, позволяющие пользователю изменить учетную запись или вызвать другую службу с учетными данными, убедитесь, что все эти взаимодействия осуществляются по протоколу HTTPS. Взаимодействия для тестирования включают следующее: 

- Формы, которые позволяют пользователям обрабатывать забытый пароль или другие учетные данные 
- Формы, которые позволяют пользователям редактировать учетные данные 
- Формы, требующие аутентификации пользователя у другого поставщика (например, обработка платежей) 

## Доступ к ресурсам во время входа в систему 

После входа в систему вы получите доступ ко всем функциям приложения, включая общедоступные функции, для доступа к которым не обязательно требуется вход в систему. Принудительный переход к HTTP-версии веб-сайта, чтобы проверить, не утекает ли клиент учетные данные. 

Тест проходит успешно, если все взаимодействия отправляют токен сеанса через HTTPS, как в следующем примере: 

```
Request URL:http://www.example.org/
Request method:GET
...
Request headers:
GET / HTTP/1.1
Host: www.example.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: keep-alive
Cookie: JSESSIONID.a7731d09=node01ai3by8hip0g71kh3ced41pmqf4.node0; ACEGI_SECURITY_HASHED_REMEMBER_ME_COOKIE=dXNlcmFiYzoxNjAyNTUwNzQ0NDU3OjFmNDlmYTZhOGI1YTZkYTYxNDIwYWVmNmM0OTI1OGFhODA3Y2ZmMjg4MDM3YjcwODdmN2I2NjMwOWIyMDU3NTc=; screenResolution=1920x1200
Upgrade-Insecure-Requests: 1

```

- Токен сеанса в файле cookie зашифрован, поскольку URL-адрес запроса — HTTPS. 

Тест не пройден, если браузер отправляет токен сеанса через HTTP в любой части веб-сайта, даже если для запуска этого случая требуется принудительный просмотр: 

```
Request URL:http://www.example.org/
Request method:GET
...
Request headers:
GET / HTTP/1.1
Host: www.example.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; screenResolution=1920x1200; JSESSIONID.c1e7b45b=node01warjbpki6icgxkn0arjbivo84.node0
Upgrade-Insecure-Requests: 1

``` 
   - Запрос GET выставил токен сеанса `JSESSIONID`(из браузера на сервер) в URL-адресе запроса `http://www.example.org/`

## Исправление 

Используйте HTTPS для всего веб-сайта. Внедрите HSTS и перенаправьте любой HTTP на HTTPS. Сайт получает следующие преимущества от использования HTTPS для всех своих функций: 

- Это предотвращает изменение взаимодействия злоумышленников с веб-сервером (включая размещение вредоносного ПО JavaScript через скомпрометированный маршрутизатор ). 
- Это позволяет избежать потери клиентов из-за предупреждений о небезопасных сайтах. Новые браузеры помечают веб-сайты на базе HTTP как небезопасные . 
- Это упрощает написание определенных приложений. Например, API-интерфейсы Android нуждаются в переопределениях для подключения к чему-либо через HTTP. 

Если переключение на HTTPS затруднительно, сначала отдайте предпочтение HTTPS для конфиденциальных операций. В среднесрочной перспективе планируйте перевести все приложение на HTTPS, чтобы не потерять клиентов из-за компрометации или предупреждений о небезопасности HTTP. Если организация еще не покупает сертификаты для HTTPS, загляните в Let's Encrypt или другие бесплатные центры сертификации на сервере. 

    
    







