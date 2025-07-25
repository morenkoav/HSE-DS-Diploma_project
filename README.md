## Определение вероятности покупки товара на основе данных о клиенте и его покупательской истории с помощью методов машинного обучения
### Итоговый проект программы «Специалист по Data Science»

**Цель проекта:** Продемонстрировать освоение ключевых методов анализа данных и их приминение на практике на основе задачи определения вероятности совершения покупки на основе данных о клиенте и его покупательской истории

**Задачи проекта:** 
1. Провести предобработку и исследовательский анализ данных полученного датасета
2. Составить портрет покупателя
3. Провести кластеризацию покупателей
4. Выбрать и обучить модель определения вероятности покупки товара


**Исходные данные:**
* Датасет ["Superstore Marketing Campaign Dataset"](https://www.kaggle.com/datasets/ahsan81/superstore-marketing-campaign-dataset)

**Содержание отчета:**
* Загрузка датасета и знакомство с данными
* Предобработка данных
    * Приведение названий столбцов к нижнему регистру
    * Приведение типов данных к целевым
    * Обработка пропусков данных
    * Обработка дубликатов данных
    * Дообогащение данных
    * Выводы по предобработке данных
* Исследовательский анализ данных
    * Описательная статистика данных
        * Признак response
        * Признак year_birth
        * Признак education
        * Признак marital_status
        * Признак income
        * Признак kidhome
        * Признак teenhome
        * Признак children_total
        * Признак is_parent
        * Признак recency
        * Признак complain
        * Признак mntwines
        * Признак mntfruits
        * Признак mntmeatproducts
        * Признак mntfishproducts
        * Признак mntsweetproducts
        * Признак mntgoldprods
        * Признак mnt_total
        * Признак expenses_per_member
        * Признак avg_check
        * Признак numdealspurchases
        * Признак numwebpurchases
        * Признак numcatalogpurchases
        * Признак numstorepurchases
        * Признак total_purchases
        * Признак numwebvisitsmonth
        * Признак registration_age
        * Вывод по разделу описательной статистики данных
    * Средние значения признаков по группам принявших и не принявших предложение о покупке клиентов
    * Матрица корреляций признаков
    * Выводы по исследовательскому анализу данных
* Выбор и обучение модели для определения вероятности покупки
    * Постановка задачи
    * Выбор базового алгоритма для обучения модели
    * Выбор метрик для анализа качества обученных моделей
    * Кодирование категориальных признаков
    * Обучение базовых моделей
         * Обучение базовых моделей без снижения размерности и обработки несбалансированности классов
            * Разбиение данных на обучающую и валидационную выборки
            * Стандартизация данных
            * Обучение моделей
        * Обучение базовых моделей с учетом снижения размерности и без обработки дисбаланса классов
            * Разбиение данных на обучающую и валидационную выборки
            * Стандартизация данных
            * Снижение размерности данных
            * Обучение моделей
        * Обучение базовых моделей с учетом снижения размерности и c обработкой дисбаланса классов
            * Изменение весов классов
            * Применение метода SMOTE для обработки дисбаланса классов
        * Обучение базовых моделей c обработкой дисбаланса классов методом оверсэмплинга
        * Обучение базовых моделей c обработкой дисбаланса классов методом ADASYN
        * Обучение базовых моделей c обработкой дисбаланса классов методом SMOTEK
        * Выбор baseline-модели
    * Подбор гиперпараметров и финальное обучение базовой модели
        * Подбор гиперпараметров базовой модели
        * Обучение модели с подобранными параметрами
        * Анализ значимости признаков для базовой модели
    * Обучение альтернативной модели на базе CATBoost
        * Обучение модели на базе CATBoost без оверсэмплинга
            * Подбор оптимальных параметров для обучения модели CATBoost (без оверсэмплинга) с помощью Grid Search
            * Обучение модели CATBoost (без оверсэмплинга) с подобранными параметрами
        * Обучение модели на базе CATBoost c оверсэмплингом
            * Подбор оптимальных параметров для обучения модели CATBoost (c оверсэмплингом) с помощью Grid Search
            * Обучение модели CATBoost (c оверсэмплингом) с подобранными параметрами
        * Анализ значимости признаков модели
    * Сравнение полученных моделей
    * Выводы по обучению моделей для определения вероятности совершения покупки клиентом
* Кластеризация клиентов
    * Постановка задачи
    * Выбор алгоритма
    * Построение дендрограммы и опредление количества кластеров
    * Обучение модели кластеризации
    * Выводы по кластеризации клиентов
* Составление портрета клиента      
    * Выводы по составлению портрета клиентов
* Общие выводы по результатам работы


### Общие выводы по результатам работы
#### <ins>**Предобработка данных**<ins>

В результате выполненной предобработки:
* Поле `dt_customer` приведено к типу даты
* Удалено 24 записи с пропусками по полю income
* Удален 201 дубликат, обнаружена ошибка с задвоенными записями и разными идентификаторами пользователей, что могло быть связано с технической ошибкой при сборе данных или при регистрации клиентов.
* Удалены три записи с аномальным годом рождения клиента (<1900 года)
* Объем данных сократился на 10% (умеренные потери)
* Добавлены шесть новых признаков:
    * `registration_age` - возраст клиента на момент регистрации в программе лояльности
    * `is_parent` - обобщенный признак наличия детей
    * `mnt_total` - общая сумма покупок по основным товарным категориям
    * `expenses_per_member` - сумма покупок в расчете на каждого члена семьи
    * `total_purchases` - общее количество покупок по разным каналам
    * `avg_check` - средний чек покупки (общая сумма / общее кол-во покупок)

#### <ins>**Исследовательский анализ данных**<ins>

**Описательная статистика данных**
* Большая часть клиентов имеют уровень образования Graduation (1014 клиентов) - выпускники средней школы или колледжа / бакалавры. Вторая по популярности категория - PhD (436 клиентов). Меньше всего клиентов с базовым уровнем образования (49).

* В разрезе семейного статуса больше всего клиентов находящихся в браке (781 человек), состоящих в отношениях (509 человек), и не состоящих в отношениях (435 человек). Самые малочисленные категории: Alone, Absurd и YOLO.

* Доходы клиентов варьируются от 1730 до 667 тыс. долларов в год. Медианный доход - 51.5 тыс долл./год, средний доход выше медианного за счет наличия нескольких экстремальных значений. Годовой доход 75% клиентов не превышает 68.6 тыс. долл.

* Больше всего клиентов, имеющих одного ребенка (1023), на втором месте клиенты без детей - 568, три ребенка - самое редкое явление - всего 45 клиентов.

* `complain` -  признак, указывающий на факт жалоб со стороны клиента. Клиентов, которые направляли жалобы всего 19 человек (менее 1%). Такой признак не несет информации, так как по сути является в 99% нулевым столбцом.

* Есть группа клиентов, у которых траты в рассмотренных категориях значительно превышают средний уровень. Это может быть полезным признаком при проведении кластеризации клиентов. Скорее всего это могут быть либо состоятельные клиенты либо клиенты имеющие большую семью.

* Покупка товаров непосредственно в магазине - наиболее популярный способ совершения покупок (около 6 покупок на клиента). Второй по популярности канал - покупки на сайте компании (около 4 покупок на клиента).

* В среднем клиенты посещают веб-сайт компании около 5 раз в месяц.

* Чаще всего регистрировались клиенты в возрасте 34-50 лет и 54-62 года. Средний возраст клиента на момент регистрации - 44 года. Самому младшему клиенту на момент регистрации было 16 лет, самому старшему 73 года.


**Средние значения признаков по группам принявших и не принявших предложение о покупке клиентов**
Для краткости обозначим клиентов, совершающих целевое действие (принимающих предложение и совершающих покупку) обозначим "группа 1", клиентов, которые не совершают целевых действий обозначим "группа 0".

**Существенные различия по группам видны у следующих показателей:**
* Группа показателей состава семьи:
    * Клиенты являющиеся родителями, имеющие большее количество маленьких детей и подростков чаще попадают в группу 0

* Группа показателей расходов на основные товарные категории:
    * Клиенты из группы 1 в среднем тратят боольше денег на основные товарные категории.
    * Средние расходы в расчете на каждого члена семьи также выше у клиентов из группы 1 (801 против 355 долл.)

* Группа показателей каналов продаж:
    * клиенты из группы 1 чаще заказывают товары по каталогу (4 против 2)
    * клиенты из группы 1 чаще покупают на сайте (5 против 4)
    * клиенты из группы 1 в целом чаще совершают покупки (15 против 12)

* У клиентов из группы 1 средний чек выше (52 долл. против 31)
* Клиенты из группы 1 чаще совершают покупки - 1 раз в 35 дней, у клиентов из группы 0  - 1 р в 51 день.
* Клиенты из группы 1 зарабатывают в среднем на 10 тыс.долл в год больше.

**Видимые отличия отсутсвуют или незначительны по следующим признакам**:
* жалобы
* покупки со скидкой
* покупки непосредственно в магазине
* количество посещений сайта за последний месяц
* возраст клиента на момент регистрации
* год рождения

**Корреляция признаков датасета**

* Чем выше доходы клиента тем выше траты на все товарные категории и количество совершенных покупок по всем каналам
* Клиенты с детьми чаще совершают покупки со скидкой
* Не удалось обнаружить положительной корреляции между целевой переменной и другими признаками в датасете
* Признак родительства и особенно наличие маленьких детей находится в отрицательной связи с доходами, тратами на основные товарные категории и прочими производными от них показателями.
* Не удалось обнаружить значимой отрицательной корреляции целевой переменной с другими признаками датасета.
* Ни один признак в датасете не имеет значимой корреляции с целевой переменной `response`.


#### <ins>**Выбор и обучение модели для определения вероятности покупки**<ins>

* В качестве базовых моделей отобраны алгоритмы:
    * Логистическая регрессия
    * KNN (K-Nearest Neighbors)
    * Метод опорных векторов (SVM) с вероятностной интерпретацией
    * Наивный байесовский классификатор
    * Дерево принятия решений для классификации
    * Случайный лес для классификации (ансамбль)
    * Градиентный бустинг для классификации (ансамбль)

* Для оценки качества прогнозов обученных моделей выбраны метрики:
    * Accuracy
    * F-1
    * ROC-AUC
* ROC-AUC хорошо подходит для несбалансированных классов (как в случае с нашим датасетом), AUC позволяет сравнивать различные модели, независимо от их порогов.

**Кодирование категориальных признаков**
* Для кодирования категориальных признаков использован метод One-Hot Encoding. В результате кодирования категориальных признаков получены 13 новых бинарных признаков.

**Обучение базовых моделей**
* Для выбора наиболее подходящей базовой модели проведены следующие эксперименты:
    * Обучение на данных "как есть" (без снижения размерности данных и обработки дисбаланса классов)
    * Обучение базовых моделей с учетом снижения размерности и без обработки дисбаланса классов
    * Обучение базовых моделей с учетом снижения размерности и c обработкой дисбаланса классов
        * Изменение весов классов встроенными методами библиотеки sklearn не повлияло на метрики решающего дерева и случайного леса, ухудшило метрки логистической регрессии, повысило метрики для SVC.
        * Примнение оверсэмплинга (SMOTE) к данным с пониженной размерностью позволило повысить качество моделей KNN, SVC, на модели решающего дерева, случайного леса и логистической регрессии повлияло слабо.
        * Применение метода оверсемплинга к данным без снижения размерности позволило повысить качество работы всех моделей кроме наивного байеса.
     * В качестве базовой модели выбрана модель градиентного бустинга с оверсэмплингом, так как она получил самую высокую суммарную оценку, а также показала высокие результаты по Accuracy (0.86), F1 (0.55) и ROC-AUC (0.85). 
* Для обучения выбранной модели проведен подбор параметров с помощью GridSearsch.
* Наиболее важными признаками для предсказания совершения покупки по модели градиентного бустинга с оверсжмплингом и подобранными параметрами получились:
    * `numcatalogpurchases` - количество покупок по каталогу (неочевидная зависимость исходя из результатов исследовательского анализа данных и анализа матрицы корреляций)
    * `numwebvisitsmonth` - количество посещений сайта за последний месяц (выглядит логично, если клиент часто заходит на сайт, явно есть какая-то заинтересованность, для таких пользователей нужно вовремя выдать УТП и подтолкнуть к покупке.)
    * `recency` - количество дней, прошедших с последней покупки
    * `teenhome` - количество детей-подростков
* Наименее важными признаками оказались признаки связанные с уровнем образования и семейным статусом.

**Обучение альтернативной модели на базе CATBoost**
* Обучение CATBoost без оверсэмплинга
    * Accuracy снизился с 0.89 до 0.87
    * F1-score снизился c 0.58 до 0.35
    * ROC-AUC увеличился 0.85 до 0.86
* Обучение CATBoost c оверсэмплингом
    * Accuracy увеличился с 0.87 до 0.97
    * F1-score увеличился c 0.35 до 0.96
    * ROC-AUC увеличился 0.86 до 0.999
    * Оверсэмплинг позволил критически улучшить все метрики одной и той же модели.
* Наиболее значимыми признаками для предсказания метки класса в моделе CATBoost с оверсэмплингом являются:
    * `recency` - был в перечне важных признаков в базовой моделе, но здесь вышел на первое место
    * `marital_status` - в базовой модели данный признак слабо влиял на результат предсказания
    * `mntgoldprods` - траты на товары с "золотой полки"
    * `numwebvisitsmonth` - был в перечне важных признаков в базовой моделе
* Наименее важными оказались признаки связанные с количеством детей, жалобами, уровнем образования.

**Сравнение и выбор финальной модели**
* В лидеры по всем метрикам с большим отрывом вышла модель на базе CATBoost с оверсэмплингом для обработки несбалансированности классов.
* Для предсказания вероятности покупкли клиентами предлагается использовать модель CATBoost с оверсэмплингом.

#### <ins>**Кластеризация клиентов**<ins>
* Для определения количества кластеров построили матрицу расстояний на основе нашего датасета с дополнительными признаки и с учетом стандартизации данных. После этого построили дендрограмму для иерархической кластеризации клиентов.
* На дендрограмме получили два кластера.
* Обучили модель на основе алгоритма K-Means, получили метки кластеров. Получили значение метрики силуэта 0.22, что говорит о приемлемом уровне разделения кластеров.
* В результате кластеризации получили 2 класса с распределением 39% - класс 1, 61% - класс 0., что значительно отличается от распределения по целевой переменной (`response`), где положительный класс составлял 15%, отрицательный - 85%.

#### <ins>**Составление портрета покупателя**<ins>
* Клиентов из кластера 1 можно охарактеризовать как активных образованных и состоятельных, тратят значительно больше на все основные товарные категории по сравнению с клиентами из кластера 0. Активно пользуются всеми каналами продаж - сайт, каталог, офлайновый магазин. Клиенты из данного кластера редко имеют более одного ребенка.
* Клиенты из кластера 0 менее активны, меньше зарабатывают, меньше тратят и реже совершают покупки. Соответственно ниже средний чек, средние траты на члена семьи. Клиенты из данного кластера часто имеют несколько детей.

Целевым сегментом для компани являются активные обеспеченные клиенты, необходимо фокусироваться на их удержании и поддержании активности за счет формирования уникальных предложений, премиального обслуживания, специальных акций. Также необходимо привлекать новых клиентов, которые по характеристикам соответствуют данному сегменту.

По клиентам из менее активного сегмента необходимо повышать их активность за счет предложения скидок, специальных акций, возможно сделать акцент на предложениях семейного формата и товары для детей и подростков.