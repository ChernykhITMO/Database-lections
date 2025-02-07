Отношение - уникальность строчек

Не всегда, когда мы делаем в SQL операции, он не всегда делает новое отношение

### Унарные операции 

**Проекция** - унарная операция, которая из отношения создаёт новое отношение, содержащее вертикальные подмножества исходного отношения

Проекция подразумевает, что строки дубликаты убираются

Проекция - вертикальная выборка, т.е. отдельные столбцы, исключая дублирования

**Выборка** - получение нового отношения, которое содержит только кортежи, удовлетворяющие некоторому условию - предикату (т.е. горизонтальная выборка)
Выбирая только часть новых кортежей, мы не нарушаем правила отношений 

### Бинарные операции

**Объединение** - Пусть есть отношение **R** и **S**, мы получим все строки, которые есть в R + которые есть в S + все строки, которые есть в R и S, за исключением дублирования

**Разность** между отношениями **R** и **S** — это множество строк из отношения **R**, которые отсутствуют в отношении **S**

Объединение и разность должны работать на отношениях, которые удовлетворяют правилу совместности, т.е. у них у них должно быть одинаковое количество атрибутов (столбцов) и домены (типы данных с ограничениями) должны совпадать

**Пересечение** - это отношение, которое определяет кортежи, которые есть одновременно и в **R** и в **S**

**Декартово прозведение** - конкатенации
Соединяем все возможные варианты двух отношение **R** и **S**
*Полное декартовое произведение* - всё со всем

**Тета соединение** - соединение, которое представляет собой подмножество кортежей из полного декартового произведения, удовлетворяющее определённому условию - предикату

Предикат может быть любой
Пример: o.productId = o.product and productid > 100

Если тета соединение делается по признаку равенства, то такое соединение называется **эквивалентным**

**Естественное соединение** - соединение эквивалентности, которое выполняется по общим атрибутам, т.е. это Inner Join

Пример: соединяем таблицу Product и SubCategory 
Общие атрибуты ProductSubCategoryID

**Левое соединение** - такое соединение по эквивалентности, когда включаются не только R строки из таблицы R левой, которые имеют значение равной атрибутам из S, но и те строки, где в общем столбце R стоит NULL, т.е. это Left Join

Таблица R (Employees):

| EmployeeID | Name  | DepartmentID |
| ---------- | ----- | ------------ |
| 1          | Иван  | 101          |
| 2          | Петр  | 102          |
| 3          | Мария | 103          |
Таблица S (Departments):

| DepartmentID | DepartmentName |
| ------------ | -------------- |
| 101          | Маркетинг      |
| 102          | IT             |

 Результат:

| Name  | DepartmentName |
| ----- | -------------- |
| Иван  | Маркетинг      |
| Петр  | IT             |
| Мария | NULL           |
**Полусоединение** — это операция, которая возвращает только строки из таблицы R, для которых существуют соответствующие строки в таблице S, удовлетворяющие условию соединения. В отличие от обычного соединения, полусоединение не включает столбцы из таблицы S в результат, а только проверяет наличие совпадений

Таблица R (Employees):

|EmployeeID|Name|DepartmentID|
|---|---|---|
|1|Иван|101|
|2|Петр|102|
|3|Мария|103|

Таблица S (Departments):

| DepartmentID | DepartmentName |
| ------------ | -------------- |
| 101          | Маркетинг      |
| 102          | IT             |

Результат:

|EmployeeID|Name|DepartmentID|
|---|---|---|
|1|Иван|101|
|2|Петр|102|

**Деление** - ситуация, когда два отношения R и S и берём из отношения R подмножество кортежей, которые определены на атрибуте C, соответствующей комбинацией всех кортежей С, где С множество атрибутов имеющихся в отношение R, но отсутствующих в отношение S (JOIN)

Операция **деления** в реляционной алгебре используется для нахождения таких кортежей в отношении R, которые **связаны со всеми кортежами** в отношении S по общим атрибутам.

- Пусть есть два отношения R(A,B) и S(B), где A и B — множества атрибутов.
- **Деление** R÷S возвращает множество всех значений атрибута A, которые связаны с **каждым** значением атрибута B из отношения S.

Отношение R

|Student|Course|
|---|---|
|Иван|Математика|
|Иван|Физика|
|Петр|Математика|
|Мария|Математика|
|Мария|Физика|

Отношение S

|Course|
|---|
|Математика|
|Физика|

Мы хотим найти студентов, которые взяли **все** курсы из таблицы S

Результат

|Student|
|---|
|Иван|
|Мария|
- Студенты **Иван** и **Мария** записаны на оба курса, указанных в таблице S ("Математика" и "Физика"). Поэтому они включены в результат деления.
- Студент **Петр** не включен, так как он записан только на "Математику", но не на "Физику".

На основании этой идеи был создан язык SQL
Язык не процедурный и не комплируемый

Основная идея, описываем какие данные хотим получить, но не описываем, как хотим получать

Язык состоит из ключевых слов и параметров:

Делится на несколько больших групп:

Язык, который определяет архитектуру (Data Definition Language)
(Create Table, Create Database и тд)

**DML** (Data Manipulation Language) - язык манипулирования данных

Существует 4 операции: Insert, Update, Delete, Select 

Оператор Select представляет собой следующую конструкцию:
  
```
SELECT [DISTINCT | ALL] { * | Col.Name[as Name], [...]][InTo Name]
[FROM] TName [INNER | LEFT OUTER | FULL][JOIN] OTable [as new]
ON
[WHERE][COND]
[GROUP BY][ColLIst(exp)]
[HAVING][COND]
[ORDER BY][Col.LiST/exp][ASC | DESC]
```

DISTINCT - мы выбираем не повторяющиеся данные, если SELECT выкинул 3 раза Color = red, то DISTINCT оставит только один

Если SELECT возвращает два столбца, color, size, то DISTINCT выдаст все возможные сочетания пар цвет размер, которые присутствовали в нашей таблице (уникальные пары)

Есть проблема: Когда получается цвет - красный, то получаем не конкретную строку, где цвет - красный, а значение - красный, т.е. мы не привязываемся к конкретным данным

ALL - до какого-то времени был обязательным, потом выпал

AS NewName - AS новое название столбца (украшательство)

InTo Name - в стандарте майкрософта, дальше результат выполнения селекта будем депонирован в новую таблицу

Если поставит в InTo "#Name" - то получится временная таблица
Две решётки - глобально временная, т.е. доступна мне и соседу
Существует, пока поддержено соединение 
Но на самом деле таблицы будут храниться на жёстком диске

TName - место **источника данных**
На этом месте может быть таблица, временная таблица, табличная переменная, ОТВ, представление и результат выполнения SELECT

INNER | LEFT OUTER | FULL][JOIN] - Тип соединения

Псевдонимы облегают жизнь, особенно, когда мы работаем больше, чем с одной таблицей
Псевдонимы обязательны при использовании коррелирующих  запросов (подзапрос, который использует значения из внешнего запроса для своей работы)
Псевдонимы полезны для чтения 
```
[ColLIst(exp)]
```
Можем делать группировку  не только по конкретной колонке, но и по результату выполнения определённой функции над этой колонкой

Пример: Хотим получать продажи по году 
```
[GROUP BY][Year(date)]
```

Если хотим получить по месяцам, то сначала группируем по году, а потом по месяцам

Чем выше уровень группировки (больше вложенности), тем медленее будет запрос

```
[ORDER BY][Col.LiST(exp)][ASC | DESC]
```
Можем упорядочивать по столбцу, либо результату выполнения операции над этим столбцом

По умолчанию - ASC (по возрастанию)
DESC - по убыванию

PL SQL - Oracle 
T-SQL и тд - разные диалекты, в зависимости от диалекта, может нагружаться разными смысловыми нагрузками

Но это наша конструкция выше - это база для всех языков

Существуют функции:

Аггрегирующие - функции, которые выполняются над группой значений атрибутов (т.е. над каким-то столбцом целиком)
MAX() - выбирает максимальное значение в столбце или подгруппе

Агреггирующие можно применять и без GROUP BY, но как правило применяются с GROUP BY

Статистические функции - всякие дисперсии, распределения и тд
Применяются с конструкцией OVER 

Язык SQL слабенько поддерживает статистические методы, поэтому лучше всего выгрузить файл данных и использовать Python или R

R - язык, которые ориентирован на работы с данными

Порядок выполнения запроса: 
SQL процедурный язык(C - процедурный), но и не скриптовый(HTML - скриптовый) 
IOS, Android - имеет в корнях язык разметки HTML 

SQL не 100 процентно скриптовый, т.е. сначала запрос анализируется, потом строится план выполнения запроса(набор процедур операций, который SQL сервер должен выполнить для увеличения производительности своего продукта)

Есть общие правила, порядок исполнения команд: 
```
1. FROM
2. ON 
3. JOIN
4. WHERE 
5. GROUP BY
6. HAVING 
7. SELECT 
8. DISTINCT
9. ORDER BY
```
Сначала проверяется соединение, потом идёт выборка данных, после этого выбираем нужные строки, далее 5, 6, 7 - формируем лист вывода, отбрасываем дубликаты и выводим упорядоченный лист вывода.

Тонкость, которую никто никогда не расскажет, если только мы не будем читать документацию SQL

Первый запрос: соединили восемь таблиц и вывели на экран название продукта, название кастомера и кол-во продуктов, которые он покупал
Данные будут выбираться из всех таблиц, которые вошли в это соединение, после этого мы соединили все таблицы и вывели на экран номер продукта и его цену, у нас будет затронута только одна таблица, но есть куча JOIN'ов

Если есть **индексы**, то у нас будут дёргаться только те таблицы, которые есть в SELECT листе, все остальные дёргаться не будут. 
Для этого и нужен SQL сервер, который будет убирать наши оплошности.


### Соединения 
Для каждой строки `[r]` из `[PT] - Parent Table` 
`[CT] - Child Table`
```
foreach[R] из [PT] 
	foreach[S] из [CT]
		if (R, S, условие)
			Вывод (R, S)
```

Это написана операция JOIN с точки зрения программирования

Если есть операция "проверка на равенство", то строки из дочерней таблицы и из S могут быть сгруппированы по какому-то признаку

Во всех СУБД, когда создаём таблицу, SQL сервер начинает думать и каким-то образом структурировать строки

На физическом уровне логичнее всего структурировать по первичному ключу

### 
[B дерево](https://neerc.ifmo.ru/wiki/index.php?title=B-%D0%B4%D0%B5%D1%80%D0%B5%D0%B2%D0%BE)

Проблема бинарного дерева: мы привыкли работать с бинарным деревом в оперативной памяти, но оно может храниться на жёстком диске

Операции чтения с жёсткого диска в современном мире работают так, что мы не можем считывать 1 байт или 10, байты считываются большими блоками. К примеру, SSD диск принципиально отдаёт большими блоками => данные хранить лучше по блокам, через ссылки. Т.е. будут блоки, которые содержать упорядоченные наборы строк, и поиск их и считывание будут простыми. 
Данные в физическом смысле будут храниться на каждом листе в конце, сама таблица будет храниться в виде дереве - это называется **класстерный B индекс** (идея применяется в компании майкроссофт)

Как только будет делать операцию JOIN с несколькими таблицами, фактически будет выборка с отдельными кусочками, поэтому этот внутренний `foreach` будет выполняться быстрее в разы.

Для того, чтобы к примеру, искать по имени, строится ещё одна группа индексов, где на конечном уровне будет храниться признак Name и будет хранить адрес строки в первом дереве, т.е. на каждую таблицу построить хоть сколько индексов, первый индекс основной будет хранить таблицу, а второй будет хранить адреса, индексировав их какому-то признаку(Имя, Фамилия, Дата рождения и тд)
Можно индексировать Фамилию и имя, тогда можно будет сравнивать по двум индексам. 

Селективность - количество одинаковых значений в столбце
Если имя редкое, то лучше индексировать по нему, индексировать по полу не имеет особо смысла, потому что индексов +- поровну будет.

Если селективность не даёт большого разнообразия данных, то индексы не имеет смысл строить 

Индексы строит администратор, СУБД их реализует 

Есть ситуация, когда индексы плохо влияют на жизнь системы, это происходит, когда у нас часто массово изменяются данные (вставка, удаления и тд)

Если такая массовость происходит, легче снести индексы и заново индексировать 

При массовых изменениях, индексы тормозят систему


### Вложенные подзапросы
Делятся на 2 блока

#### 1. Простые вложенные подзапросы
```
SELECT
FROM T1
WHERE ... = (SELECT ..... FROM T2 ...)
```
Вложенный подзапрос формирует значение или набор значений, которые будут сохранены в КЭШе и будут рассматриваться, как обособленный закэшированный набор данных, поэтому вложенные подзапросы обычно работают быстро

#### 2. Коррелирующие подзапросы
```
SELECT ...
FROM T1 
WHERE ... = (
SELECT .. 
FROM T2 
WHERE T1.Col = T2.Col)
```
Есть таблица верхнего уровня, где мы говорим, что будем выбирать только те строки, для которых некоторые значения удовлетворяют условию

Такие запросы нельзя кэшировать, потому что значение для верхнего уровня мы получим только рассмотрев строчку внутри другого отношения

Коррелирующие подзапросы работают медленно, в связи с чем, являются дорогим ресурсом

Так же могут быть такими:

```
SELECT ..., [SELECT ..... T1.Col = T2.Col]  
from T1
```






