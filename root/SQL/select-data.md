# Команда GROUP BY

---

Команда GROUP BY позволяет группировать результаты при выборке из базы данных.
К сгруппированным результатам можно применять любые функции.
См. также команду <mark>having</mark>, 
которая позволяет накладывать условие на группы, созданные с помощью <mark>group by</mark>.

```sql
SELECT * FROM имя_таблицы 
WHERE условие 
GROUP BY поле_для_группировки
```

| id  | name  | age  | salary |
|-----|-------|------|--------|
|1    |	Дима  |	23   |	100   |
|2    |	Петя  |	23   |	200   |
|3    |	Вася  |	23   |	300   |
|4    |	Коля  |	24   |	1000  |
|5    |	Иван  |	24   |	2000  |
|6    |	Кирилл|	25   |	1000  |

### Пример
В данном примере записи группируются по возрасту (будет 3 группы - 23 года, 24 года и 25 лет). 
Затем для каждой группы применяется функция sum, которая суммирует зарплаты внутри данной группы.

В результате для каждой из групп (23 года, 24 года и 25 лет) будет подсчитана суммарная зарплата внутри этой группы:

```sql
SELECT age, SUM(salary) as sum 
FROM workers 
GROUP BY age
```

| age  | sum    |
|------|--------|
|23    |	600 |
|24    |	3000|
|25    |	1000|

[old.code.mu](http://old.code.mu/sql/group-by.html)

---

# Выборка данных с созданием вычисляемого столбца

---

С помощью SQL запросов можно осуществлять вычисления по каждой строке таблицы с помощью вычисляемого столбца. 
Для него в списке полей после оператора SELECT указывается выражение и задается имя.

> Выражение может включать: имена столбцов, константы, знаки операций, встроенные функции.

Результатом является таблица, в которую включены все данные из указанных после SELECT столбцов, 
а также новый столбец, в каждой строке которого вычисляется заданное выражение.

```sql
SELECT title, author, price, amount, 
     price * amount AS total 
FROM book;
```

| title                 | author           | price  | amount | total   |
|-----------------------|------------------|--------|--------|---------|
| Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      | 2012.97 |
| Белая гвардия         | Булгаков М.А.    | 540.50 | 5      | 2702.50 |
| Идиот                 | Достоевский Ф.М. | 460.00 | 10     | 4600.00 |
| Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      | 1598.02 |
| Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     | 9750.00 |

---

# Вычисляемые столбцы, математические функции

---

В SQL реализовано множество математических функций для работы с числовыми данными. 
В таблице приведены некоторые из них.

|Функция   |Описание   |Пример   |
|----------|-----------|---------|
|CEILING(x)	|возвращает наименьшее целое число, большее или равное x (округляет до целого числа в большую сторону)|	CEILING(4.2)=5   CEILING(-5.8)=-5|
|<mark>ROUND(x, k)</mark>|округляет значение x до k знаков после запятой,если k не указано – x округляется до целого|ROUND(4.361)=4   ROUND(5.86592,1)=5.9|
|FLOOR(x)|возвращает наибольшее целое число, меньшее или равное x(округляет до  целого числа в меньшую сторону)|FLOOR(4.2)=4   FLOOR(-5.8)=-6|
|POWER(x, y)|возведение x в степень y|POWER(3,4)=81.0|
|SQRT(x)|квадратный корень из x|SQRT(4)=2.0   SQRT(2)=1.41...|
|DEGREES(x)|конвертирует значение x из радиан в градусы|DEGREES(3) = 171.8...|
|RADIANS(x)|конвертирует значение x из градусов в радианы|RADIANS(180)=3.14...|
|ABS(x)|модуль числа x|ABS(-1) = 1   ABS(1) = 1|
|PI()|pi = 3.1415926...||


---

# Логические функции

---

В SQL реализована возможность заносить в поле значение в зависимости от условия. 
Для этого используется <mark>функция IF</mark>:

```sql
IF(логическое_выражение, выражение_1, выражение_2)
```

Функция вычисляет логическое_выражение, если оно истина – в поле заносится значение выражения_1, 
в противном случае – значение выражения_2. Все три параметра IF() являются обязательными.

(Допускается использование вложенных функций, вместо выражения_1 или выражения_2 может стоять новая функция IF)

*Пример 1*

*Для каждой книги из таблицы book установим скидку следующим образом: 
если количество книг меньше 4, то скидка будет составлять 50% от цены, в противном случае 30%.*

```sql
SELECT title, amount, price, 
    IF(amount<4, price*0.5, price*0.7) AS sale
FROM book;
```
*Пример 2*

*Усложним вычисление скидки в зависимости от количества книг. 
Если количество книг меньше 4 – то скидка 50%, меньше 11 – 30%, 
в остальных случаях – 10%. И еще укажем какая именно скидка на каждую книгу.*

```sql
SELECT title, amount, price,
    ROUND(IF (amount < 4, price * 0.5, IF (amount < 11, price * 0.7, price * 0.9)), 2) AS sale,
    IF (amount < 4, 'скидка 50%', IF (amount < 11, 'скидка 30%', 'скидка 10%')) AS Ваша_скидка
FROM book;
```

*Пример 3*

*При анализе продаж книг выяснилось, что наибольшей популярностью пользуются книги Михаила Булгакова, 
на втором месте книги Сергея Есенина. Исходя из этого решили поднять цену книг Булгакова на 10%, 
а цену книг Есенина – на 5%. Написать запрос, куда включить автора, название книги и новую цену, 
последний столбец назвать new_price. Значение округлить до двух знаков после запятой.*

```sql
SELECT author, title, 
ROUND( 
    IF(author = 'Булгаков М.А.', price * 1.1, IF(author = 'Есенин С.А.', price * 1.05, price)
    ), 2) AS new_price
FROM book;
```
```sql
SELECT author, title,
CASE
    WHEN author = 'Булгаков М.А.' THEN ROUND(price * 1.1 , 2)
    WHEN author = 'Есенин С.А.' THEN ROUND(price * 1.05, 2)
    ELSE price
END AS new_price
FROM book;
```

---

# Выборка данных по условию

---

Логическое выражение может включать операторы сравнения
<mark>(равно «=», не равно «<>», больше «>», меньше «<», больше или равно«>=», меньше или равно «<=»)</mark>
и выражения, допустимые в SQL.

```sql
SELECT title, author, price * amount AS total
FROM book
WHERE price * amount > 4000;
```
В логическом выражении после WHERE нельзя использовать названия столбцов, присвоенные им с помощью AS, 
так как при выполнении запроса <mark>сначала вычисляется логическое выражение для каждой строки исходной таблицы, 
выбираются строки, для которых оно истинно. 
А только после этого формируется "шапка запроса" – столбцы, включаемые в запрос</mark>.











### [to main page](../../README.md)