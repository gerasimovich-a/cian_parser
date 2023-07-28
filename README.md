### Парсер объявлений недвижимости с сайта Циан

Данный скрипт:
- собирает информацию из поисковой выдачи по заданным параметрам 
- НЕ заходит на страницу каждого объявления, а собирает нужную информацию непосредственно из поисковой выдачи 
(тем самым значительно ускоряется время работы, но теряются некоторые данные. частично этот недостаток компенсируется тем, что нужная информация извлекается из параметров запроса)
- упаковывает результаты в словарь


Логика работы парсера построена таким образом, чтобы обойти ограничение Циана на выдачу в 54 страницы путем формирования сетки запросов по параметрам:

- количество комнат
- этаж (с 1 по 49)
- последний/не последний этаж
- квартира/аппартаменты

При тестировании на мск скрипт собрал 99_512 уникальных объявлений из ~130_000 за 4 часа и 18 минут работы

Пример собранной информации

|           | link                                     |   rooms |   floor | is_apartment   | not_last_floor   | title                              | subtitle                            | deadline                  | metro             | metro_remote          | geo                                                                          |   main_price | currency   |   price_for_sq_m |
|----------:|:-----------------------------------------|--------:|--------:|:---------------|:-----------------|:-----------------------------------|:------------------------------------|:--------------------------|:------------------|:----------------------|:-----------------------------------------------------------------------------|-------------:|:-----------|-----------------:|
| 290090154 | https://www.cian.ru/sale/flat/290090154/ |       1 |       1 | True           | True             | 1-комн. апарт., 40,05 м², 1/5 этаж | Сдача корпуса 4 кв. 2024            | сдача ГК: 3 кв. 2024 года | Площадь Революции | 2 минуты пешком       | Москва, ЦАО, р-н Тверской, м. Площадь Революции, Ильинка 3/8 ЖК              |     82040000 | rouble     |          2048439 |
| 283849683 | https://www.cian.ru/sale/flat/283849683/ |       1 |       1 | True           | True             | Новая студия 12,5 кв.м             | 1-комн. апарт., 12,5 м², 1/5 этаж   | nan                       | Волжская          | 5 минут пешком        | Москва, ЮВАО, р-н Текстильщики, м. Волжская, улица Шкулева, 13/25С3          |      4000000 | rouble     |           320000 |
| 270246372 | https://www.cian.ru/sale/flat/270246372/ |       1 |       1 | True           | True             | 1-комн. апарт., 37,3 м², 1/8 этаж  | Секция 1 • Сдача корпуса 2 кв. 2024 | сдача ГК: 2 кв. 2024 года | Петровский Парк   | 5 минут пешком        | Москва, САО, р-н Савеловский, м. Петровский Парк, улица Верхняя Масловка, 20 |     14346459 | rouble     |           384624 |
| 288719837 | https://www.cian.ru/sale/flat/288719837/ |       1 |       1 | True           | True             | Новая студия на Западе Москвы      | 1-комн. апарт., 14,4 м², 1/17 этаж  | nan                       | Кунцевская        | 5 минут на транспорте | Москва, ЗАО, р-н Можайский, м. Кунцевская, Дорогобужская улица, 3            |      4100000 | rouble     |           284722 |
| 287829364 | https://www.cian.ru/sale/flat/287829364/ |       1 |       1 | True           | True             | 1-комн. апарт., 39 м², 1/8 этаж    | Сдача корпуса 2 кв. 2024            | сдача ГК: 2 кв. 2024 года | Петровский Парк   | 5 минут пешком        | Москва, САО, р-н Савеловский, м. Петровский Парк, улица Верхняя Масловка, 20 |     16125603 | rouble     |           413477 |


### Геокодирование
Проведено геокодирование метро и жд платформ
Геокодирование осуществлялось с помощью API OSM Nominatim (несколько станций определены неверно и требуют ручных правок). По найденным координатам построена визуализация медианной цены за кв. метр в зависимости от местоположения ближайшего метро

Карта цен
![img](https://github.com/gerasimovich-a/cian_parser/blob/main/price_map.PNG?raw=true)


Планируемые доработки
- извлечение доп информации из описания
- извлечение доп информации из данных геолокации
- увеличение параметров для сетки запросов для сбора большего количества данных

