ВАРИАНТ 6.
=========
Уровень 1
---------
1. Дана схема базы данных в виде следующих отношений. С помощью операторов SQL создать логическую структуру соответствующих таблиц для хранения в СУБД, используя известные средства поддержания целостности (NOT NULL, UNIQUE, и т.д.). Обосновать выбор типов данных и используемые средства поддержания целостности. При выборе подходящих типов данных использовать информацию о конкретных значениях полей БД (см. прил.1)

ПОТРЕБИТЕЛЬ
ИДЕНТИФИКАТОР | НАЗВАНИЕ | АДРЕС ЖИТЕЛЬСТВА | СКИДКА,%
--------------|----------|------------------|---------

ПОСТАВЩИК
ИДЕНТИФИКАТОР  |ФАМИЛИЯ  |АДРЕС  |КОММИСИОННЫЕ,%
---------------|---------|-------|--------------

ЗАКАЗ
НОМЕР  |ДАТА  |ПОТРЕБИТЕЛЬ  |ПОСТАВЩИК  |ДЕТАЛЬ  |КОЛ-ВО  |СУММА, РУБ
-------|------|-------------|-----------|--------|--------|----------

ДЕТАЛЬ
ИДЕНТИФИКАТОР  |НАИМЕНОВАНИЕ  |СКЛАД  |КОЛ-ВО  |ЦЕНА, РУБ
---------------|--------------|-------|--------|---------

```SQL
CREATE TABLE `ПОТРЕБИТЕЛЬ` (
  `ИДЕНТИФИКАТОР` int(11) NOT NULL AUTO_INCREMENT,
  `НАЗВАНИЕ` varchar(255) NOT NULL,
  `АДРЕС ЖИТЕЛЬСТВА` varchar(255) NOT NULL,
  `СКИДКА, %` int(11) NOT NULL,
  PRIMARY KEY(`ИДЕНТИФИКАТОР`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `ПОСТАВЩИК` (
  `ИДЕНТИФИКАТОР` int(11) NOT NULL AUTO_INCREMENT,
  `ФАМИЛИЯ` varchar(20) NOT NULL,
  `АДРЕС` varchar(30) NOT NULL,
  `КОММИСИОННЫЕ,%` int(11) NOT NULL,
  PRIMARY KEY(`ИДЕНТИФИКАТОР`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `ЗАКАЗ` (
  `НОМЕР` int(11) NOT NULL AUTO_INCREMENT,
  `ДАТА` varchar(20) NOT NULL,
  `ПОТРЕБИТЕЛЬ` int(11) NOT NULL,
  `ПОСТАВЩИК` int(11) NOT NULL,
  `ДЕТАЛЬ` int(11) NOT NULL,
  `КОЛ-ВО` int(11) NOT NULL,
  `СУММА, РУБ` int(11) NOT NULL,
  PRIMARY KEY(`НОМЕР`),
  CONSTRAINT ЗАКАЗ_ПОТРЕБИТЕЛЬ
  FOREIGN KEY (ПОТРЕБИТЕЛЬ) REFERENCES `ПОТРЕБИТЕЛЬ` (`ИДЕНТИФИКАТОР`) 
  ON UPDATE RESTRICT
  ON DELETE RESTRICT,
  CONSTRAINT ЗАКАЗ_ПОСТАВЩИК
  FOREIGN KEY (`ПОСТАВЩИК`) REFERENCES `ПОСТАВЩИК` (`ИДЕНТИФИКАТОР`) 
  ON UPDATE RESTRICT
  ON DELETE RESTRICT,
  CONSTRAINT ЗАКАЗ_ДЕТАЛЬ
  FOREIGN KEY (`ДЕТАЛЬ`) REFERENCES `ДЕТАЛЬ` (`ИДЕНТИФИКАТОР`) 
  ON UPDATE RESTRICT
  ON DELETE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `ДЕТАЛЬ` (
  `ИДЕНТИФИКАТОР` int(11) NOT NULL AUTO_INCREMENT,
  `НАИМЕНОВАНИЕ` varchar(20) NOT NULL,
  `СКЛАД` varchar(20) NOT NULL,
  `КОЛ-ВО` int(11) NOT NULL,
  `ЦЕНА, РУБ` int(11) NOT NULL,
  PRIMARY KEY(`ИДЕНТИФИКАТОР`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE ПОТРЕБИТЕЛЬ CHECK (`СКИДКА, %` <= 100 AND `СКИДКА, %` >= 0);
ALTER TABLE ПОСТАВЩИК CHECK (`КОММИСИОННЫЕ,%` <= 100 AND `КОММИСИОННЫЕ,%` >= 0);
ALTER TABLE ЗАКАЗ CHECK (`КОЛ-ВО` >= 0 AND `СУММА, РУБ` > 0);
ALTER TABLE ДЕТАЛЬ CHECK (`КОЛ-ВО` >= 0 AND `ЦЕНА, РУБ` > 0);
```
2. Ввести в ранее созданные таблицы конкретные данные (см. прил. 1). Использовать скрипт-файл из операторов INSERT или вспомогательную утилиту .
```SQL
INSERT INTO `ПОТРЕБИТЕЛЬ` (`ИДЕНТИФИКАТОР`, `НАЗВАНИЕ`, `АДРЕС ЖИТЕЛЬСТВА`, `СКИДКА, %`) VALUES
(1, 'АО ВАРЯ', 'Сормовский', 10),
(2, 'ГАЗ', 'Автозаводский', 7),
(3, 'МП ВЕРА', 'Канавинский', 5),
(4, 'МП', 'Канавинский', 3),
(5, 'АО СТАЛЬ', 'Советский', 0);

INSERT INTO `ПОСТАВЩИК` (`ИДЕНТИФИКАТОР`, `ФАМИЛИЯ`, `АДРЕС`, `КОММИСИОННЫЕ,%`) VALUES
(1, 'Артюхина', 'Сормовский', 4),
(2, 'Щепин', 'Приокский', 4),
(3, 'Власов', 'Канавинский', 5),
(4, 'Кузнецова', 'Советский', 5),
(5, 'Цепилева', 'Нижегородский', 3),
(6, 'Корнилов', 'Нижегородский', 6);

INSERT INTO `ЗАКАЗ` (`НОМЕР`, `ДАТА`, `ПОТРЕБИТЕЛЬ`, `ПОСТАВЩИК`, `ДЕТАЛЬ`, `КОЛ-ВО`, `СУММА, РУБ`) VALUES
(1, 'Январь', 5, 4, 3, 7, 21000),
(2, 'Февраль', 3, 3, 3, 2, 6000),
(3, 'Февраль', 4, 5, 4, 200, 180000),
(4, 'Март', 5, 4, 2, 50, 50000),
(5, 'Апрель', 1, 6, 7, 110, 132000),
(6, 'Апрель', 4, 4, 1, 150, 750000),
(7, 'Май', 2, 4, 6, 20, 40000),
(8, 'Июнь', 1, 3, 7, 2000, 2400000),
(9, 'Июнь', 2, 5, 7, 10000, 12000000),
(10, 'Июнь', 3, 6, 1, 5, 25000),
(11, 'Июнь', 4, 3, 3, 1, 3000),
(12, 'Июнь', 4, 4, 1, 10, 50000),
(13, 'Июль', 1, 6, 6, 3, 6000),
(14, 'Июль', 2, 1, 2, 1000, 1000000),
(15, 'Июль', 2, 2, 1, 100, 5000000),
(16, 'Июль', 5, 1, 5, 100, 15000),
(17, 'Август', 1, 4, 7, 12000, 24400000);

INSERT INTO `ДЕТАЛЬ` (`ИДЕНТИФИКАТОР`, `НАИМЕНОВАНИЕ`, `СКЛАД`, `КОЛ-ВО`, `ЦЕНА, РУБ`) VALUES
(1, 'Втулка', 'Сормовский', 20000, 5000),
(2, 'Болт', 'Сормовский', 40000, 1000),
(3, 'Ключ гаечный', 'Канавинский', 5000, 3000),
(4, 'Шпилька', 'Автозаводский', 10000, 900),
(5, 'Винт', 'Сормовский', 50000, 1500),
(6, 'Молоток', 'Канавинский', 1200, 2000),
(7, 'Шуруп', 'Сормовский', 30000, 1200);
```

3. Используя оператор SELECT создать запрос для вывода всех строк каждой таблицы. Проверить правильность ввода. При необходимости произвести коррекцию значений операторами INSERT, UPDATE, DELETE.
```sql
SELECT * FROM ПОТРЕБИТЕЛЬ;
```
| ИДЕНТИФИКАТОР | НАЗВАНИЕ | АДРЕС ЖИТЕЛЬСТВА | СКИДКА, % |
|---------------|----------|------------------|-----------|
|             1 | АО ВАРЯ  | Сормовский       |        10 |
|             2 | ГАЗ      | Автозаводский    |         7 |
|             3 | МП ВЕРА  | Канавинский      |         5 |
|             4 | МП       | Канавинский      |         3 |
|             5 | АО СТАЛЬ | Советский        |         0 |

```sql
SELECT * FROM ПОТРЕБИТЕЛЬ;
```
| ИДЕНТИФИКАТОР | ФАМИЛИЯ   | АДРЕС         | КОММИСИОННЫЕ,% |
|---------------|-----------|---------------|----------------|
|             1 | Артюхина  | Сормовский    |              4 |
|             2 | Щепин     | Приокский     |              4 |
|             3 | Власов    | Канавинский   |              5 |
|             4 | Кузнецова | Советский     |              5 |
|             5 | Цепилева  | Нижегородский |              3 |
|             6 | Корнилов  | Нижегородский |              6 |
```sql
SELECT * FROM ЗАКАЗ;
```
| НОМЕР | ДАТА    | ПОТРЕБИТЕЛЬ | ПОСТАВЩИК | ДЕТАЛЬ | КОЛ-ВО | СУММА, РУБ |
|-------|---------|-------------|-----------|--------|--------|------------|
|     1 | Январь  |           5 |         4 |      3 |      7 |      21000 |
|     2 | Февраль |           3 |         3 |      3 |      2 |       6000 |
|     3 | Февраль |           4 |         5 |      4 |    200 |     180000 |
|     4 | Март    |           5 |         4 |      2 |     50 |      50000 |
|     5 | Апрель  |           1 |         6 |      7 |    110 |     132000 |
|     6 | Апрель  |           4 |         4 |      1 |    150 |     750000 |
|     7 | Май     |           2 |         4 |      6 |     20 |      40000 |
|     8 | Июнь    |           1 |         3 |      7 |   2000 |    2400000 |
|     9 | Июнь    |           2 |         5 |      7 |  10000 |   12000000 |
|    10 | Июнь    |           3 |         6 |      1 |      5 |      25000 |
|    11 | Июнь    |           4 |         3 |      3 |      1 |       3000 |
|    12 | Июнь    |           4 |         4 |      1 |     10 |      50000 |
|    13 | Июль    |           1 |         6 |      6 |      3 |       6000 |
|    14 | Июль    |           2 |         1 |      2 |   1000 |    1000000 |
|    15 | Июль    |           2 |         2 |      1 |    100 |    5000000 |
|    16 | Июль    |           5 |         1 |      5 |    100 |      15000 |
|    17 | Август  |           1 |         4 |      7 |  12000 |   24400000 |
```sql
SELECT * FROM ДЕТАЛЬ;
```
| ИДЕНТИФИКАТОР | НАИМЕНОВАНИЕ | СКЛАД         | КОЛ-ВО | ЦЕНА, РУБ |
|---------------|--------------|---------------|--------|-----------|
|             1 | Втулка       | Сормовский    |  20000 |      5000 |
|             2 | Болт         | Сормовский    |  40000 |      1000 |
|             3 | Ключ гаечный | Канавинский   |   5000 |      3000 |
|             4 | Шпилька      | Автозаводский |  10000 |       900 |
|             5 | Винт         | Сормовский    |  50000 |      1500 |
|             6 | Молоток      | Канавинский   |   1200 |      2000 |
|             7 | Шуруп        | Сормовский    |  30000 |      1200 |
  
4. Создать запросы для вывода:
a) всех различных размеров комиссионных;
```sql
SELECT DISTINCT `КОММИСИОННЫЕ,%` FROM ПОСТАВЩИК;
```
| КОММИСИОННЫЕ,% |
|----------------|
|              4 |
|              5 |
|              3 |
|              6 |

b) всех различных фамилий поставщиков;
```sql
SELECT DISTINCT `ФАМИЛИЯ` FROM ПОСТАВЩИК;
```
| ФАМИЛИЯ   |
|-----------|
| Артюхина  |
| Щепин     |
| Власов    |
| Кузнецова |
| Цепилева  |
| Корнилов  |

c) всех различных наименований деталей.
```sql
SELECT DISTINCT `НАИМЕНОВАНИЕ` FROM ДЕТАЛЬ;
```
| НАИМЕНОВАНИЕ |
|--------------|
| Втулка       |
| Болт         |
| Ключ гаечный |
| Шпилька      |
| Винт         |
| Молоток      |
| Шуруп        |

5. Создав запрос получить следующую информацию:
a) фамилии и адреса поставщиков, имеющих размер комиссионных менее 5%;
```sql
SELECT ФАМИЛИЯ, АДРЕС,`КОММИСИОННЫЕ,%` FROM ПОСТАВЩИК WHERE `КОММИСИОННЫЕ,%` < 5;
```
| ФАМИЛИЯ  | АДРЕС         | КОММИСИОННЫЕ,% |
|----------|---------------|-----------------|
| Артюхина | Сормовский    |              4 |
| Щепин    | Приокский     |              4 |
| Цепилева | Нижегородский |              3 |

b) название и адрес склада для деталей, находящихся в количесиве менее 1500 шт.;
```sql
SELECT СКЛАД, `КОЛ-ВО`, НАИМЕНОВАНИЕ FROM ДЕТАЛЬ WHERE `КОЛ-ВО` < 1500;
```
| СКЛАД       | КОЛ-ВО | НАИМЕНОВАНИЕ |
|-------------|--------|--------------|
| Канавинский |   1200 | Молоток      |

c) название, адрес и размер скидки для предприятий, имеющих в названии слово “МП”. Отсортировать по адресу и размеру скидки.
```sql
SELECT НАЗВАНИЕ, `АДРЕС ЖИТЕЛЬСТВА`, `СКИДКА, %` FROM ПОТРЕБИТЕЛЬ WHERE НАЗВАНИЕ LIKE 'МП%' ORDER BY `АДРЕС ЖИТЕЛЬСТВА`,`СКИДКА, %`;
```
| НАЗВАНИЕ | АДРЕС ЖИТЕЛЬСТВА | СКИДКА, % |
|----------|------------------|-----------|
| МП       | Канавинский      |         3 |
| МП ВЕРА  | Канавинский      |         5 |

6. На основании данных о заказах вывести все данные в таком формате:
a) номер, дата, фамилия поставщика, сумма заказа. Отсортировать по фамилиям и сумме заказа;
```sql
SELECT НОМЕР, ДАТА, ФАМИЛИЯ, `СУММА, РУБ` FROM ЗАКАЗ, ПОСТАВЩИК WHERE ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР ORDER BY ФАМИЛИЯ, 'СУММА, РУБ';
```
| НОМЕР | ДАТА    | ФАМИЛИЯ   | СУММА, РУБ |
|-------|---------|-----------|------------|
|    14 | Июль    | Артюхина  |    1000000 |
|    16 | Июль    | Артюхина  |      15000 |
|     8 | Июнь    | Власов    |    2400000 |
|     2 | Февраль | Власов    |       6000 |
|    11 | Июнь    | Власов    |       3000 |
|    10 | Июнь    | Корнилов  |      25000 |
|     5 | Апрель  | Корнилов  |     132000 |
|    13 | Июль    | Корнилов  |       6000 |
|     1 | Январь  | Кузнецова |      21000 |
|    17 | Август  | Кузнецова |   24400000 |
|    12 | Июнь    | Кузнецова |      50000 |
|     7 | Май     | Кузнецова |      40000 |
|     4 | Март    | Кузнецова |      50000 |
|     6 | Апрель  | Кузнецова |     750000 |
|     3 | Февраль | Цепилева  |     180000 |
|     9 | Июнь    | Цепилева  |   12000000 |
|    15 | Июль    | Щепин     |    5000000 |

b) название детали, количество, дата.
```sql
SELECT НАИМЕНОВАНИЕ, ЗАКАЗ.`КОЛ-ВО`, ДАТА FROM ЗАКАЗ, ДЕТАЛЬ WHERE ЗАКАЗ.ДЕТАЛЬ = ДЕТАЛЬ.ИДЕНТИФИКАТОР;
```
| НАИМЕНОВАНИЕ | КОЛ-ВО | ДАТА    |
|--------------|--------|---------|
| Ключ гаечный |      7 | Январь  |
| Ключ гаечный |      2 | Февраль |
| Шпилька      |    200 | Февраль |
| Болт         |     50 | Март    |
| Шуруп        |    110 | Апрель  |
| Втулка       |    150 | Апрель  |
| Молоток      |     20 | Май     |
| Шуруп        |   2000 | Июнь    |
| Шуруп        |  10000 | Июнь    |
| Втулка       |      5 | Июнь    |
| Ключ гаечный |      1 | Июнь    |
| Втулка       |     10 | Июнь    |
| Молоток      |      3 | Июль    |
| Болт         |   1000 | Июль    |
| Втулка       |    100 | Июль    |
| Винт         |    100 | Июль    |
| Шуруп        |  12000 | Август  |

7. Вывести:
a) названия и размер скидки организаций-потребителей, куда поставлял детали Щепин, а общая сумма заказа превышала 5000;
```sql
SELECT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ, ПОТРЕБИТЕЛЬ.`СКИДКА, %`, ПОСТАВЩИК.ФАМИЛИЯ, ЗАКАЗ.`СУММА, РУБ` FROM ПОТРЕБИТЕЛЬ, ПОСТАВЩИК, ЗАКАЗ WHERE ПОСТАВЩИК.ФАМИЛИЯ LIKE 'Щепин' AND ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР AND ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР GROUP BY ПОТРЕБИТЕЛЬ.НАЗВАНИЕ, ПОТРЕБИТЕЛЬ.`СКИДКА, %`, ПОСТАВЩИК.ФАМИЛИЯ, ЗАКАЗ.`СУММА, РУБ` HAVING SUM(ЗАКАЗ.`СУММА, РУБ`) > 5000;
```
| НАЗВАНИЕ | СКИДКА, % | ФАМИЛИЯ | СУММА, РУБ |
|----------|-----------|---------|------------|
| ГАЗ      |         7 | Щепин   |    5000000 |

b) фамилии и размер комиссионных для поставщиков, поставлявших детали предприятиям чужих районов не ранее января. Отсортировать по возрастанию комиссионных; 
из условия следует, что все месяца входят, но мы решили, что не включая январь
```sql
SELECT DISTINCT ПОСТАВЩИК.ФАМИЛИЯ, ПОСТАВЩИК.`КОММИСИОННЫЕ,%` FROM ПОСТАВЩИК, ЗАКАЗ, ПОТРЕБИТЕЛЬ WHERE ЗАКАЗ.ДАТА NOT LIKE 'Январь' AND ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР AND ПОСТАВЩИК.АДРЕС != ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА`;
```
| ФАМИЛИЯ   | КОММИСИОННЫЕ,% |
|-----------|----------------|
| Власов    |              5 |
| Цепилева  |              3 |
| Кузнецова |              5 |
| Корнилов  |              6 |
| Артюхина  |              4 |
| Щепин     |              4 |

c) название и оставшееся количество деталей, заказывавшихся в количестве более 2 штук предприятиями Автозаводского и Советского районов. В вывод добавить суммарную стоимость соответствующих заказов;
```sql
SELECT ДЕТАЛЬ.НАИМЕНОВАНИЕ, ДЕТАЛЬ.`КОЛ-ВО`, SUM(ЗАКАЗ.`СУММА, РУБ`) FROM ДЕТАЛЬ, ПОТРЕБИТЕЛЬ, ЗАКАЗ WHERE ЗАКАЗ.`КОЛ-ВО` > 2 AND (ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` LIKE 'Автозаводский' OR ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` LIKE 'Советский') GROUP DY ДЕТАЛЬ.НАИМЕНОВАНИЕ, ДЕТАЛЬ.`КОЛ-ВО`;
```
d) названия предприятий одного района, заказывавших молотки.
```SQL
SELECT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ FROM ПОТРЕБИТЕЛЬ, ЗАКАЗ, ДЕТАЛЬ WHERE ДЕТАЛЬ.НАИМЕНОВАНИЕ LIKE 'Молоток' AND ЗАКАЗ.ДЕТАЛЬ = ДЕТАЛЬ.ИДЕНТИФИКАТОР AND ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР AND ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` IN (SELECT `АДРЕС ЖИТЕЛЬСТВА` FROM ПОТРЕБИТЕЛЬ GROUP BY `АДРЕС ЖИТЕЛЬСТВА` HAVING COUNT(`АДРЕС ЖИТЕЛЬСТВА`) > 1);
```
EMPTY SET
8.Cздать запрос для модификации всех значений столбца с суммарной величиной платы, чтобы он содержал истинную сумму, которую заплатил потребитель ( с учетом скидки). Вывести новые значения.
```sql
UPDATE ЗАКАЗ SET `СУММА, РУБ` = `СУММА, РУБ` - `СУММА, РУБ` * (SELECT `СКИДКА, %` FROM ПОТРЕБИТЕЛЬ WHERE ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР) / 100;
SELECT `СУММА, РУБ` FROM ЗАКАЗ;
```
| СУММА, РУБ |
|------------|
|      21000 |
|       5700 |
|     174600 |
|      50000 |
|     118800 |
|     727500 |
|      37200 |
|    2160000 |
|   11160000 |
|      23750 |
|       2910 |
|      48500 |
|       5400 |
|     930000 |
|    4650000 |
|      15000 |
|   21960000 |
9. Расширить таблицу с данными о заказах столбцом, содержащим величину комиссионных поставщика. Создать запрос для ввода конкретных значений во все строки таблицы.
```SQL
ALTER TABLE ЗАКАЗ ADD `КОММИСИОННЫЕ, РУБ` INT NOT NULL;
UPDATE ЗАКАЗ SET `КОММИСИОННЫЕ, РУБ` = `СУММА, РУБ` * (SELECT `КОММИСИОННЫЕ,%` FROM ПОСТАВЩИК WHERE ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР) / 100
SELECT `КОММИСИОННЫЕ, РУБ` FROM ЗАКАЗ;
```
| КОММИСИОННЫЕ, РУБ |
|-------------------|
|              1050 |
|               285 |
|              5238 |
|              2500 |
|              7128 |
|             36375 |
|              1860 |
|            108000 |
|            334800 |
|              1425 |
|               146 |
|              2425 |
|               324 |
|             37200 |
|            186000 |
|               600 |
|           1098000 |

Уровень 2
---------
10. Используя операцию IN (NOT IN) реализовать следующие запросы:
a) найти всех потребителей, заказывавших болты или винты не менее двух раз;
```SQL
SELECT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ FROM ПОТРЕБИТЕЛЬ, ЗАКАЗ, ДЕТАЛЬ WHERE ДЕТАЛЬ.НАИМЕНОВАНИЕ IN ('Болт', 'Винт') AND ДЕТАЛЬ.ИДЕНТИФИКАТОР = ЗАКАЗ.ДЕТАЛЬ AND ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР GROUP BY ПОТРЕБИТЕЛЬ.НАЗВАНИЕ HAVING COUNT(*) >= 2;
```
| НАЗВАНИЕ |
|----------|
| АО СТАЛЬ |

b) найти потребителей, не делавших заказов на сумму менее 500000руб. поставщикам из своего района ;
```SQL
SELECT DISTINCT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ FROM ПОТРЕБИТЕЛЬ, ПОСТАВЩИК, ЗАКАЗ WHERE ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` IN (ПОСТАВЩИК.АДРЕС) AND ЗАКАЗ.ПОТРЕБИТЕЛЬ IN (ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР) AND ЗАКАЗ.ПОСТАВЩИК IN (ПОСТАВЩИК.ИДЕНТИФИКАТОР) GROUP BY ПОТРЕБИТЕЛЬ.НАЗВАНИЕ HAVING SUM(ЗАКАЗ.`СУММА, РУБ`) >= 500000;
```
EMPTY SET
c) запросы задания 7.с и 7.d.

```SQL
SELECT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ FROM ПОТРЕБИТЕЛЬ, ЗАКАЗ, ДЕТАЛЬ WHERE ДЕТАЛЬ.НАИМЕНОВАНИЕ IN ('Молоток') AND ЗАКАЗ.ДЕТАЛЬ IN (ДЕТАЛЬ.ИДЕНТИФИКАТОР) AND ЗАКАЗ.ПОТРЕБИТЕЛЬ IN (ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР) AND ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` IN (SELECT `АДРЕС ЖИТЕЛЬСТВА` FROM ПОТРЕБИТЕЛЬ GROUP BY `АДРЕС ЖИТЕЛЬСТВА` HAVING COUNT(`АДРЕС ЖИТЕЛЬСТВА`) > 1);
```
EMPTY SET

11. Используя операции ALL-ANY реализовать следующие запросы:

a) найти поставщика с наименьшими комиссионными, который в мае поставлял детали потребителю, сделавшему заказ максимальной стоимости в апреле;

b) найти деталь у которой цена совпадает с ценой какой-либо ( но не той же самой) детали, проданной поставщиком из Советского района с максимальными комиссионными;

c) найти потребителя, который имеет не максимальный размер скидки и покупал детали у поставщиков из Канавинского района;

d) запрос задания 7.а.

12. Используя операцию UNION получить места складирования деталей и места расположения поставщиков.

13. Используя операцию EXISTS ( NOT EXISTS ) реализовать нижеследующие запросы. В случае, если для текущего состояния БД запрос будет выдавать пустое множество строк, требуется указать какие добавления в БД необходимо провести.

a) определить потребителей, заказывавших детали с ценой более 6000руб. у всех поставщиков из Советского или Канавинского районов;

b) найти деталь, которую заказывали в количестве одной штуки все потребители;

c) какие детали не заказывали потребители с размером скидки менее 5%;

d) найти потребителя, заказывавшего все детали, не поставляемые поставщиками из Сормовского района.

14. Реализовать запросы с использованием аггрегатных функций:
a) определить суммарную стоимость всех заказов, произведенных потребителями из Канавинского района;
```SQL
SELECT SUM(ЗАКАЗ.`СУММА, РУБ`) FROM ЗАКАЗ, ПОТРЕБИТЕЛЬ WHERE ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР AND ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` IN ('Канавинский');
```
| SUM(ЗАКАЗ.`СУММА, РУБ`) |
|-------------------------|
|                  982960 |

b) найти среднее число заказываемых деталей со ценой более 2000;
```SQL
SELECT AVG(ЗАКАЗ.`КОЛ-ВО`) FROM ЗАКАЗ, ДЕТАЛЬ WHERE ДЕТАЛЬ.`ЦЕНА, РУБ`>2000 AND ЗАКАЗ.ДЕТАЛЬ = ДЕТАЛЬ.ИДЕНТИФИКАТОР;
```
| AVG(ЗАКАЗ.`КОЛ-ВО`) |
|---------------------|
|             39.2857 |

c) найти максимальную скидку среди потребителей, заказывавших детали у поставщиков из своего района;
```SQL
SELECT MAX(ПОТРЕБИТЕЛЬ.`СКИДКА, %`) FROM ПОТРЕБИТЕЛЬ, ЗАКАЗ, ПОСТАВЩИК WHERE ПОСТАВЩИК.АДРЕС = ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` AND ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР AND ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР;
```
| MAX(ПОТРЕБИТЕЛЬ.`СКИДКА, %`) |
|------------------------------|
|                            5 |

d) какие детали имеют цену за штуку меньше средней.
```SQL
SELECT ДЕТАЛЬ.НАИМЕНОВАНИЕ FROM ДЕТАЛЬ WHERE ДЕТАЛЬ.`ЦЕНА, РУБ` < (SELECT AVG(`ЦЕНА, РУБ`) FROM ДЕТАЛЬ);
```
| НАИМЕНОВАНИЕ |
|--------------|
| Болт         |
| Шпилька      |
| Винт         |
| Молоток      |
| Шуруп        |

15. Используя средства группировки реализовать следующие запросы:
a) найти для каждой пары “потребитель-поставщик” суммарную величину стоимости произведенных заказов;
```SQL
SELECT ПОТРЕБИТЕЛЬ.НАЗВАНИЕ, ПОСТАВЩИК.ФАМИЛИЯ, SUM(ЗАКАЗ.`СУММА, РУБ`) FROM ЗАКАЗ, ПОТРЕБИТЕЛЬ, ПОСТАВЩИК WHERE ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР AND ЗАКАЗ.ПОСТАВЩИК = ПОСТАВЩИК.ИДЕНТИФИКАТОР GROUP BY ПОТРЕБИТЕЛЬ.НАЗВАНИЕ, ПОСТАВЩИК.ФАМИЛИЯ;
```
| НАЗВАНИЕ | ФАМИЛИЯ   | SUM(ЗАКАЗ.`СУММА, РУБ`) |
|----------|-----------|-------------------------|
| АО ВАРЯ  | Власов    |                 2160000 |
| АО ВАРЯ  | Корнилов  |                  124200 |
| АО ВАРЯ  | Кузнецова |                21960000 |
| АО СТАЛЬ | Артюхина  |                   15000 |
| АО СТАЛЬ | Кузнецова |                   71000 |
| ГАЗ      | Артюхина  |                  930000 |
| ГАЗ      | Кузнецова |                   37200 |
| ГАЗ      | Цепилева  |                11160000 |
| ГАЗ      | Щепин     |                 4650000 |
| МП       | Власов    |                    2910 |
| МП       | Кузнецова |                  776000 |
| МП       | Цепилева  |                  174600 |
| МП ВЕРА  | Власов    |                    5700 |
| МП ВЕРА  | Корнилов  |                   23750 |

b) найти детали, которые более трех раз заказывали потребители из Советского района;
```SQL
SELECT ДЕТАЛЬ.НАИМЕНОВАНИЕ FROM ЗАКАЗ, ПОТРЕБИТЕЛЬ, ДЕТАЛЬ WHERE ПОТРЕБИТЕЛЬ.`АДРЕС ЖИТЕЛЬСТВА` LIKE 'Советский' AND ЗАКАЗ.ПОТРЕБИТЕЛЬ = ПОТРЕБИТЕЛЬ.ИДЕНТИФИКАТОР AND ЗАКАЗ.ДЕТАЛЬ = ДЕТАЛЬ.ИДЕНТИФИКАТОР GROUP BY ДЕТАЛЬ.НАИМЕНОВАНИЕ HAVING COUNT(*) > 3;
```
EMPTY SET

c) найти месяц, в котором все заказы имели стоимость не менее 10000;
```SQL
SELECT ЗАКАЗ.ДАТА FROM ЗАКАЗ GROUP BY ЗАКАЗ.ДАТА ALL;
```
all,
  (
    SELECT max(cnt)
    FROM (
      SELECT year, month, sum(flag) AS cnt
      FROM
        (
          SELECT
            DATEPART(yyyy, date) AS year,
            DATEPART(mm, date) AS month,
            flag
          FROM table
        ) y_m
      GROUP BY year, month
    ) tbl
  ) max
WHERE all.cnt = max.cnt
d) получить для каждой детали со ценой более 10000 среднее количество заказываемых деталей.
У нас нет деталей со стоимостью 10000, поэтому мы взяли 1000
```SQL
SELECT ДЕТАЛЬ.НАИМЕНОВАНИЕ, ДЕТАЛЬ.`ЦЕНА, РУБ`, AVG(ЗАКАЗ.`КОЛ-ВО`) FROM ДЕТАЛЬ, ЗАКАЗ WHERE ДЕТАЛЬ.`ЦЕНА, РУБ`>1000 AND ЗАКАЗ.ДЕТАЛЬ = ДЕТАЛЬ.ИДЕНТИФИКАТОР GROUP BY ДЕТАЛЬ.НАИМЕНОВАНИЕ, ДЕТАЛЬ.`ЦЕНА, РУБ`;
```
| НАИМЕНОВАНИЕ | ЦЕНА, РУБ | AVG(ЗАКАЗ.`КОЛ-ВО`) |
|--------------|-----------|---------------------|
| Винт         |      1500 |            100.0000 |
| Втулка       |      5000 |             66.2500 |
| Ключ гаечный |      3000 |              3.3333 |
| Молоток      |      2000 |             11.5000 |
| Шуруп        |      1200 |           6027.5000 |
