1. Заповніть таблицю БД ще трьома рядками.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v1.png)

2. Створіть схему даних користувача та віртуальну таблицю у цій схемі з правилами вибіркового керування доступом для користувача так, щоб він міг побачити тільки один з рядків таблиці з урахуванням одного значення її останньої колонки.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v2.PNG)
![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v3.png)

3. Перевірте роботу механізму вибіркового керування, виконавши операції SELECT, INSERT, UPDATE, DELETE.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v4.png)
![image](![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v5.png)
![image](![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/v66.png)

Операції SELECT, INSERT, UPDATE, DELETE стають доступними, якщо користувач postgres надає права на них. Операції SELECT, UPDATE, DELETE можливо проводити тільки з 1 доступним рядком, але операція INSERT може додати у справжню таблицю human нові значення, якиї не буде у таблиці схеми при невиконанні умов, або можуть з'явитися, якщо дата народження зівпадає з умовою.
