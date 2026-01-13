![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/03d.png)  

## Среда выполнения  

- **Microsoft Office.** Версия 2508 (сборка 19127.20154). Английский язык.  
- **Microsoft Power BI Desktop**. Версия: 2.131.1203.0 (24.07) (x64). Английский язык.  


## **Файлы решения**:  
- [`solution.xlsx`](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/solution.xlsx)  
- [`solution.pbix`](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/solution.pbix)  


## Данные  

### Blinkit Marketing and Customer Feedback Dashboard  

**Автор:** Yash Motiani  

**Ссылка:** https://www.kaggle.com/datasets/yashmotiani/blinkit-marketing-and-customer-powerbi-dashbord  

**Лицензия:** Open Database License  

**Описание:** Датасет содержит данные о продажах из онлайн-магазина Blinkit. Включает 6 CSV файлов, которые содержат сведения о товарах, заказах, акциях, выручке.  

## Решение  
### Этап 1. Подготовка среды

Создание палитры цветов:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00a.png)  

Создание кастомной палитры в Excel:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00b.png)  

Примеры использования кастомной палитры:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00d.png)  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00e.png)  

[Проверка палитры цветов](https://color.adobe.com/ru/create/color-accessibility) на восприятие людьми с дальтонизмом:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00f.png)  


### Этап 2. Подготовка данных (Excel + Power Query)  

Для визуализации способа хранения данных хорошо подойдет ERD-диграмма.  

Составить диаграмму можно в приложении [drawsql](https://drawsql.app/teams/-4118/diagrams/orders-blinkit).  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00_erd.png)  

#### Импорт данных  

Загрузим все csv-файлы (6 таблиц) в Power Query.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01a.png)  

Используя Power Query, проведем очистку данных:  

- Удаление неиспользуемых данных (столбцы, таблицы).  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01b.png)  

_В таблице `customers` есть столбец `pincode`, в нём хранятся 6-тизначные цифры, похожие на незашифрованные пароли. Это является серьезной угрозой нарушению безопасности персональных данных. Если такое было бы замечено в настоящей базе данных, то следовало бы (усилием команды) исправить это и зашифровать данные._  

- Удалим пробелы вначале и конце столбцов с типом данных `текст`.  
`Transform` -> `Trim`  

- Приведение данных к одной форме. Приведите данные к нужным типам (дата, число, текст).
Частая проблема - десятичные числа в разных странах записываются по-разному. Нужно заменить `.` на `,`, изменить тип данных на десятичные.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01c.png)  

- Удалим дубликаты.  
Используем атрибуты `id` в таблицах, чтобы проверить уникальность записей. Например, в таблице `customers` это столбец `customer_id`, в таблице `orders` - `order_id`.  

Можно загрузить add-in "Python for Excel", чтобы использовать Python (включая библиотеки re для регулярных выражений или pandas для работы с матрицами).  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00python.png)  


![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/00_excel.png)  


#### Трансформация данных:

Сгруппируем  таблицы заказов и покупателей по `id` покупателей:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01d.png)  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01e.png)  

Создадим вычисляемый столбец общей суммы продажи клиентов.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/01f.png)  


### Этап 3. Анализ данных (Excel)

#### Сводные таблицы: Анализ продаж

Создадим сводную таблицу.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02a.png)  
##### Анализ динамики во времени
Проведем анализ динамики во времени

В 2023 и 2024 годах в зимняя время продажи снижались.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02b.png)  



##### Продажи по категориям и подкатегориям товаров.  
Уже ранее присоединили к `orders` таблицу `customers`, чтобы проанализировать изменения количества заказов с изменением времени. Так как данных немного, используем слияние таблиц. Также присоединим таблицу `products`, чтобы иметь подробную информацию о товарах.  

Продажи по категориям товаров:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02f.png)  


##### Продажи по регионам.  

Около 300 строк с городами. В таблице нет группировки по регионам.  
Top-10 регионов по продажам:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02g.png)  


##### Средний чек и количество товаров в заказе.  

Применим эти атрибуты в сводной таблице:  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02h.png)  

Видим, что:  
- Средний чек: 2201.  
- Среднее количество товаров в заказе: 2 шт.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02i.png)  

Предпочтения методов оплаты покупателей:  
все методы оплаты имеют близкое значение (около 1/4 каждый); не выявлено взаимосвязи между средним чеком  и способом оплаты.  

Пятерка самых часто заказываемых товаров:  
![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/02i.png)  

> Товар, который заказывают реже всего - шпинат: в 2 раза реже печенья и в 8 раз реже, чем хлеб.  



### Этап 4.  Визуализация и интерактивность в Power BI

#### Импорт данных из Excel в Power BI Desktop.  

Для того, чтобы дашборд работал быстрее:  
- используем только необходимые в отчете таблицы;  
- удалим ненужные столбцы.  

##### Моделирование данных: связи между таблицами, созданные в Excel.

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/03a.png)  

#### Визуализация в Power BI  

##### Создание интерактивного дашборда  
Дашборд включает сводные показатели: общий объем продаж, количество заказов, средний чек, количество); графики; фильтры.  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/03b.png)  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/03c.png)  

![Скриншот](https://github.com/igor-zalevskii/dashboards/blob/main/aston/screenshots/03d.png)
