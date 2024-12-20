> [Начало](../../README.md) >> Модуль 4

# DataLearn Module 4

#etl #elt



# Домашнее задание

**Модуль 4** преследует такие цели:

- Знакомство с ETL
- Исследовать какой-нибудь ETL инструмент на выбор.

---



# Pentaho Data Integration

Когда я делал Модуль 3, мне пришлось перепрыгнуть и Модуль 4 и собрать ETL для доставки данных.

В целом мне очень понравилось. Pentaho может очень много и он очень мало весит, а так-же может биг дату. Прочитал больше половины книги Kettle Solutions, которую рекомендовали в модуле и не пожалел об этом. Было очень много вопросов по типу: "Как получить строки, которые обновлены?", "Какие преобразования лучше выполнять первыми?" Решения, как оказалось были очень простые и в целом по ETL все готово. Бери и делай. ETL подсистемы - обязательны. 

Главный скриншот этого модуля. Спасибо, Pentaho Kettle Solutions, Вы ответили мне на один из самых главных вопросов "Как правильно?".

![](_att/Maxthon%20Snap20240323155108.png)


Собралось чуток забавного материала, а точнее неочевидных проблем. 

### 1

Выгрузка таблицы из источника в текстовый файл. Оказывается в ячейке есть "переход на новую строку" и лесом идет весь документ. Trim бесполезен, кавычки не помогают...

### 2

Или вот. Создаю таблицу (Postgres), но `category_id` не уникальный, а просто `SERIAL`. Ведь я же буду в нее сгружать какие-то данные. На первый раз я делаю вставку данных из существующей таблицы через `SELECT *`. Как видно, счетчик `SERIAL` не обращает внимание на то, что я туда уже загрузил. 

![](_att/Maxthon%20Snap20240323142301.png)

Но если вставку делать через явное указание столбцов `SELECT name, last_update`, то счетчик работает и `category_id` увеличивается.

![](_att/Maxthon%20Snap20240323143517.png)

### 3

Нужно следить за метаданными, что на выходе идут. Если явно не указать, то можно получить колонки по 64 килобайт каждая. Pentaho лезет в базу данных, меняет там атрибуты колонки, если они не совпадают с автоматическими настройками от ETL.

![](_att/Maxthon%20Snap20240327172923.png)

![](_att/Maxthon%20Snap20240327173417.png)

![](_att/Maxthon%20Snap20240327180011.png)

Но ниже показывает, что он генерирует SQL перед отправкой данных, но в базе данных атрибуты колонки не поменялись.. Так, что как это работает, ясно не до конца. Но я думаю, что лучше все вручную указывать явно.

![](_att/Maxthon%20Snap20240401183258.png)  

![](_att/Maxthon%20Snap20240401183825.png)



# Bonus

Можно забрать интеллект карту о [подсистемах ETL](ETL.xmind). Дополнять ее точно буду во время чтения книг про ETL (названия слева).

![](_att/ETL.png)


---

> [Начало](../../README.md) >> Модуль 4