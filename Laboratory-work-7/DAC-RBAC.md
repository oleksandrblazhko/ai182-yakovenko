1. Створіть термінальну консоль psql через утиліту командного рядка вашої ОС та встановіть з’єднання з БД postgres від імені користувача-адміністратора postgres

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f1.png)

2. Зареєструйте нового користувача в СКБД PostgreSQL, назва якого співпадає з вашим ім'ям, наприклад ivan, і довільним паролем.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f2.png)

3. Створіть роль в СКБД PostgreSQL (назва співпадає з вашим прізвищем латинськими літерами) і надайте новому користувачеві можливість наслідувати цю роль.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f3.png)

4. Створіть реляційну таблицю з урахуванням варіанту з таблиці 2.1 від імені користувача-адміністратора.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f4.png)

5. Внесіть один рядок в таблицю, використовуючи команду insert into ..., відповідно до варіанту.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f5.png)

6. Додатково створіть ще одну термінальну консоль psql та та встановіть з’єднання з БД postgres від імені нового користувача.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f6.png)

7. Від імені вашого користувача виконайте запит на отримання даних з таблиці (select * from таблиця). Запротоколюйте результат виконання команди.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f7.png)

8. Встановіть повноваження на читання таблиці новому користувачеві.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f8.png)

9. Повторіть крок 8.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f9.png)

10. Зніміть повноваження на читання таблиці для нового користувача.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/10.png)

11. Повторіть крок 8.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f11.png)

12. Створіть команду оновлення даних таблиці (UPDATE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f12.png)

13. Встановіть повноваження на оновлення таблиці новому користувачеві.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f13.png)

14. Повторіть крок 13.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f14.png)

15. Створіть команду видалення запису таблиці (DELETE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f15.png)

16. Встановіть повноваження на видалення таблиці новому користувачеві.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f16.png)

17. Повторіть крок 16.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f17.png)

18. Зніміть всі повноваження з таблиці для нового користувача.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f18.png)

19. Створіть команду внесення запису в таблицю (INSERT) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f19.png)

20. Встановіть повноваження на внесення даних до таблиці для ролі.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f20.png)

21. Повторіть крок 20.

![image](https://github.com/oleksandrblazhko/ai182-yakovenko/blob/laboratory-work-7/Laboratory-work-7/images/f21.png)
