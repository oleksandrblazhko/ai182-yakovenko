# Testing for Weak Lock Out Mechanism

## Резюме 

Механизмы блокировки учетной записи используются для смягчения атак грубой силы. Некоторые из атак, которые можно победить с помощью механизма блокировки: 

- Атака на подбор пароля или имени пользователя. 
- Угадывание кода по любым функциям 2FA или секретным вопросам. 

Механизмы блокировки учетных записей требуют баланса между защитой учетных записей от несанкционированного доступа и защитой пользователей от отказа в санкционированном доступе. Учетные записи обычно блокируются после 3–5 неудачных попыток и могут быть разблокированы только по истечении заранее определенного периода времени с помощью механизма самообслуживания или вмешательства администратора. 

Несмотря на то, что проводить атаки методом перебора легко, результат успешной атаки опасен, поскольку злоумышленник будет иметь полный доступ к учетной записи пользователя, а вместе с ней и ко всем функциям и службам, к которым у него есть доступ. 

## Цели теста 

- Оцените способность механизма блокировки учетной записи смягчить подбор пароля методом грубой силы. 
- Оцените устойчивость механизма разблокировки к несанкционированной разблокировке аккаунта. 

## Как проверить 

### Механизм блокировки 

Чтобы проверить надежность механизмов блокировки, вам потребуется доступ к учетной записи, которую вы хотите или можете себе позволить заблокировать. Если у вас есть только одна учетная запись, с помощью которой вы можете войти в веб-приложение, выполните этот тест в конце плана тестирования, чтобы не потерять время тестирования из-за блокировки. 

Чтобы оценить способность механизма блокировки учетной записи смягчить подбор пароля методом грубой силы, попытайтесь выполнить неверный вход в систему, указав неверный пароль несколько раз, прежде чем использовать правильный пароль, чтобы убедиться, что учетная запись заблокирована. Пример теста может быть следующим: 

1. Попытка входа с неверным паролем 3 раза. 
2. Успешно войдите в систему с правильным паролем, тем самым показывая, что механизм блокировки не срабатывает после 3 неправильных попыток аутентификации. 
3. Попытка войти с неверным паролем 4 раза. 
4. Успешно войдите в систему с правильным паролем, тем самым показывая, что механизм блокировки не срабатывает после 4 неправильных попыток аутентификации. 
5. Попытка войти с неверным паролем 5 раз. 
6. Попытайтесь войти с правильным паролем. Приложение возвращает «Ваша учетная запись заблокирована», тем самым подтверждая, что учетная запись заблокирована после 5 неверных попыток аутентификации. 
7. Попытайтесь войти с правильным паролем через 5 минут. Приложение возвращает «Ваша учетная запись заблокирована», тем самым показывая, что механизм блокировки не разблокируется автоматически через 5 минут. 
8. Попытайтесь войти с правильным паролем через 10 минут. Приложение возвращает «Ваша учетная запись заблокирована», тем самым показывая, что механизм блокировки не разблокируется автоматически через 10 минут. 
9. Успешно войдите в систему с правильным паролем через 15 минут, тем самым показывая, что механизм блокировки автоматически разблокируется через 10-15 минут. 

CAPTCHA может препятствовать атакам методом грубой силы, но они могут иметь свои недостатки и не должны заменять механизм блокировки. Механизм CAPTCHA можно обойти, если он реализован неправильно. Недостатки CAPTCHA включают в себя: 

1. Легко преодолеваемый вызов, например, арифметический или ограниченный набор вопросов. 
2. CAPTCHA проверяет код ответа HTTP вместо успешного ответа. 
3. Логика CAPTCHA на стороне сервера по умолчанию дает успешное решение. 
4. Результат проверки CAPTCHA никогда не проверяется на стороне сервера.
5. Поле или параметр ввода CAPTCHA обрабатывается вручную и неправильно проверяется или экранируется. 

Чтобы оценить эффективность CAPTCHA: 

1. Оцените проблемы CAPTCHA и попытайтесь автоматизировать решения в зависимости от сложности. 
2. Попытка отправить запрос без решения CAPTCHA с помощью обычных механизмов пользовательского интерфейса. 
3. Попытка отправить запрос с преднамеренной ошибкой CAPTCHA. 
4. Попытка отправить запрос без решения CAPTCHA (при условии, что некоторые значения по умолчанию могут быть переданы кодом на стороне клиента и т. д.) при использовании тестового прокси (запрос отправляется непосредственно на стороне сервера). 
5. Попытаться фаззить точки ввода данных CAPTCHA (если они есть) с помощью обычных полезных данных для внедрения или последовательностей специальных символов. 
6. Проверьте, может ли решение CAPTCHA быть альтернативным текстом изображений, имен файлов или значений в связанных скрытых полях. 
7. Попытаться повторно отправить ранее выявленные заведомо хорошие ответы. 
8. Проверьте, не приводит ли очистка файлов cookie к обходу CAPTCHA (например, если CAPTCHA отображается только после нескольких неудачных попыток). 
9. Если CAPTCHA является частью многоэтапного процесса, попробуйте просто получить доступ или выполнить шаг за пределами CAPTCHA (например, если CAPTCHA является первым шагом в процессе входа в систему, попробуйте просто отправить второй шаг [имя пользователя и пароль]). 
10. Проверьте наличие альтернативных методов, которые могут не включать CAPTCHA, например конечную точку API, предназначенную для облегчения доступа к мобильным приложениям. 

Повторите этот процесс для всех возможных функций, для которых может потребоваться механизм блокировки. 

### Разблокировать механизм 

Чтобы оценить устойчивость механизма разблокировки к несанкционированной разблокировке учетной записи, запустите механизм разблокировки и найдите слабые места. Типичные механизмы разблокировки могут включать секретные вопросы или ссылку на разблокировку по электронной почте. Ссылка для разблокировки должна быть уникальной одноразовой ссылкой, чтобы злоумышленник не мог угадать или воспроизвести ссылку, а также выполнять групповые атаки методом грубой силы. 

Обратите внимание, что механизм разблокировки следует использовать только для разблокировки учетных записей. Это не то же самое, что механизм восстановления пароля, но он может следовать тем же методам безопасности.

## Исправление 

Применять механизмы разблокировки аккаунта в зависимости от уровня риска. В порядке от низшей к высшей гарантии: 

1. Блокировка и разблокировка по времени. 
2. Самостоятельная разблокировка (отправляет электронное письмо для разблокировки на зарегистрированный адрес электронной почты). 
3. Ручная разблокировка администратором. 
4. Ручная разблокировка администратором с положительной идентификацией пользователя. 

Факторы, которые следует учитывать при реализации механизма блокировки учетной записи: 

1. Каков риск перебора пароля в приложении? 
2. Достаточно ли CAPTCHA, чтобы снизить этот риск
3. Используется ли механизм блокировки на стороне клиента (например, JavaScript)? (Если это так, отключите клиентский код для тестирования.) 
4. Количество неудачных попыток входа в систему до блокировки. Если порог блокировки слишком низкий, действительные пользователи могут быть заблокированы слишком часто. Если порог блокировки слишком высок, тем больше попыток злоумышленник может предпринять для взлома учетной записи, прежде чем она будет заблокирована. В зависимости от цели приложения типичным порогом блокировки является диапазон от 5 до 10 неудачных попыток. 
5. Как будут разблокированы аккаунты?

        1.Вручную администратором: это наиболее безопасный метод блокировки, но он может доставлять неудобства пользователям и отнимать «ценное» время администратора. 
            1. Обратите внимание, что у администратора также должен быть способ восстановления на случай, если его учетная запись будет заблокирована. 
            2. Этот механизм разблокировки может привести к атаке типа «отказ в обслуживании», если целью злоумышленника является блокировка учетных записей всех пользователей веб-приложения. 
        2. Через некоторое время: какова продолжительность блокировки? Достаточно ли этого для защищаемого приложения? Например, продолжительность блокировки от 5 до 30 минут может быть хорошим компромиссом между смягчением атак грубой силы и созданием неудобств действительным пользователям. 
        3. С помощью механизма самообслуживания: как указывалось ранее, этот механизм самообслуживания должен быть достаточно безопасным, чтобы злоумышленник не мог самостоятельно разблокировать учетные записи. 
    
