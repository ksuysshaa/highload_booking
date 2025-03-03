# Booking.com

## Содержание

- [Booking](#booking)
  - [Содержание](#содержание)
  - [Основная часть](#основная-часть)
    - [1. Тема и целевая аудитория](#1-тема-и-целевая-аудитория)
      - [Функционал MVP](#функционал-mvp)
      - [Целевая аудитория](#целевая-аудитория)
        - [Анализ трафика и вовлеченности](#анализ-трафика-и-вовлеченности)
        - [Веб-трафик по странам](#веб-трафик-по-странам)
        - [Демографические показатели](#демографические-показатели)
  - [Список источников](#список-источников)

## Основная часть

### 1. Тема и целевая аудитория

**Booking** - система интернет-бронирования отелей.

#### Функционал MVP

1. Регистрация и авторизация пользователей
2. Поиск
3. Бронирование жилья
    - Создание бронирования: форма бронирования, подтверждение на email
    - Редактирование дат бронирования (в рамках доступных)
    - Отмена бронирования
4. Кабинет владельца жилья: 
    - Размещение и редактирование объявления (фото, описание, удобства, цена, тип проживания, раcположение на карте, доступные даты)
    - Управление доступными датами
5. Кабинет пользователя:
    - Просмотр текущих бронирований
6. Добавление отзывов об объекте размещения
7. Чат между владельцем жильца и бронирующим
8. Система оплаты бронирований

#### Фичи

1. Интерактивная карта с объектами размещения
2. Система напоминаний и уведомлений
3. Поддержка гибких условий бронирования
4. Просмотр истории бронирований

#### Целевая аудитория

##### Анализ трафика и вовлеченности

- всего посещений (за месяц): **520.4M** [^1]
- доля пользователей, покинувших сайт после просмотра одной страницы: **33.14%** [^1]
- страниц на посещение: **8.23** [^1]
- средняя продолжительность посещения: **00:08:27** [^1]
- **`>120`** млн активных пользователей в месяц [^2]
- **`>1B`** ночей проживания бронируется через букинг в год [^2]

##### Веб-трафик по странам

[![Traffic by Country](images/ByCountryRating.png)](https://www.similarweb.com/ru/website/booking.com) [^2]

##### Демографические показатели

[![Demographic Indicators](images/DemographyRating.png)](https://www.similarweb.com/ru/website/booking.com) [^2]

### 2. Расчет нагрузки

#### 1. Продуктовые метрики

1.1. Аудитория

- **MAU (Monthly Active Users):** ориентировочно **400–500 млн**  

- **Пиковый DAU (Daily Active Users):** условно ~**20 млн**    

1.2. Среднее количество действий (в день, на 1 пользователя)

По официальным данным Booking Holdings, в 2022 году через их сервисы было забронировано свыше **900 млн room nights**. Делим условно эту цифру на несколько сотен миллионов активных пользователей, понимаем, что:

- **Брони** в среднем у активного пользователя 1–3 раза в год.  
- В процессе выбора жилья на 1 визит может приходиться несколько **поисков** и **просмотров** (другие объекты).

Для упрощённого расчёта, на **1 пользователя в «пиковые» сутки** возьмём следующее:

| **Действие**                       | **Кол-во в день (на пользователя)** | **Откуда цифра**                                                  |
|:-----------------------------------|:------------------------------------:|:-------------------------------------------------------------------|
| **Поиск** (города, даты, фильтры) | 10                                   | Пользователь обычно многократно меняет фильтры и даты.            |
| **Просмотр карточек отелей**       | 15                                   | При подборе жилья просматривают 10–20 вариантов.                  |
| **Создание/изменение брони**       | 0,1                                  | По статистике, не каждый день, но 1 раз в 10 дней — усреднённо.    |
| **Отзывы** (написать/прочитать)    | 0,2                                  | Меньшая часть пользователей пишет отзывы, многие — только читают. |
| **Сообщения** (чат с отелем)       | 0,2                                  | Вопросы о заселении, дополнительной информации и т.п.             |

Итого получается **~25–26 действий** на одного «действительно активного» пользователя.  

---

#### 2. Объём данных на одного пользователя

2.1. Долгосрочное хранение (за год)

| **Категория**                                          | **Оценка размера (1 пользователь за год)** | **Комментарий**                                                                                 |
|:-------------------------------------------------------|:------------------------------------------:|:-------------------------------------------------------------------------------------------------|
| **Профиль** (аватар, контакты, платёжные данные)       | ~0,5 МБ                                     | Данные учётки                                             |
| **История бронирований** (1–3 в год)                    | ~0,3 МБ                                     | Информация о транзакциях, чеки, подтверждения.                                |
| **Отзывы** (1 раз в год + метаданные)                  | ~0,1 МБ                                     | Текст отзыва + метаданные (оценка). Реже фотографии.     |
| **Сообщения** (в чате)                                 | ~0,05 МБ                                    | Небольшой текст, изредка вложения (скрин, PDF).                                                  |
| **Рекомендации, избранное**                            | ~3 МБ                                       | Кеш подборок жилья, списки «Избранное».                                              |

**Итого:** \~4 МБ на пользователя в год (0,5 + 0,3 + 0,1 + 0,05 + 3 = 3,95 ≈ 4).  

### 2.2. Сетевой трафик (за сутки, «пиковый» пользователь)

1. **Поиск** — \~0,7 МБ на 1 запрос (HTML, миниатюры фото).  
2. **Просмотр отеля** — \~2 МБ на 1 просмотр (карточка, фото, отзывы).  
3. **Создание/изменение брони** — \~0,5 МБ.  
4. **Отзыв** (просмотр/написание) — \~0,3 МБ.  
5. **Сообщение** — \~0,1 МБ.
6. **Авторизация/Регистрация** — \~0,05 МБ.  
7. **Управление объявлениями** — \~1 МБ.  

Тогда:

| **Действие**       | **Кол-во** | **Объём на 1 действие (МБ)** | **Итого на 1 пользователя/день (МБ)** | **Итого на 1 пользователя/день (Гбит/с)** |
|:-------------------|-----------:|-----------------------------:|---------------------------------------:|------------------------------------------:|
| Поиск              | 10         | 0,7                         | 7                                     | \(\frac{7 \times 8}{86400} \approx 0,00065\) |
| Просмотр отеля     | 15         | 2                           | 30                                    | \(\frac{30 \times 8}{86400} \approx 0,00278\) |
| Бронирование       | 0,1        | 0,5                         | 0,05                                  | \(\frac{0,05 \times 8}{86400} \approx 0,0000046\) |
| Отзывы             | 0,2        | 0,3                         | 0,06                                  | \(\frac{0,06 \times 8}{86400} \approx 0,0000056\) |
| Сообщения          | 0,2        | 0,1                         | 0,02                                  | \(\frac{0,02 \times 8}{86400} \approx 0,0000019\) |
| Авторизация/Регистрация | 0.1   | 0,05                        | 0,005                                 | \(\frac{0,005 \times 8}{86400} \approx 0,00000046\) |
| Управление объявлениями | 0,1   | 1                           | 0,1                                   | \(\frac{0,1 \times 8}{86400} \approx 0,0000093\) |
| **Итого**          | —          | —                           | **37,235**                            | **\(\approx 0,0028\)** |

#### 3. Итог

3.1. Общий объём хранения

1. **Персональные данные** (данные пользователей):  

    500млн×4МБ=2000ТБ=2ПБ.  

2. **Данные об отелях**:  
   - 28+ млн активных объявлений.  
   - Каждое объявление может иметь десятки высококачественных фото, описания, отзывы и т.п.  
   - Итого получается **десятки–сотни ПБ**.

3. **Исторические логи, транзакции, бэкапы**:  
   - Booking Holdings сообщала, что было свыше 900 млн ночей (room nights) забронировано за год, и это хранится для отчётности, аналитики и юридических целей.  
   - С учётом логов и резервных копий речь идёт о **десятках ПБ**.

4. **Объявления**:  
   - Включает текстовые данные, фотографии, и метаданные для каждого объявления.
   - Оценка: **десятки ПБ**.

3.2. Сетевой трафик

Возьмём **DAU = 20 млн** (пиковый день) × **37,235 МБ** на пользователя:

20млн×37,235МБ=744,7млн МБ=744700ГБ=0,745ПБ/сутки

Перевод в Гбит/с:

\[
\frac{0,745 \times 1024 \times 8}{86400} \approx 70,5 \text{ Гбит/с}
\]

Этот объём (**\~70,5 Гбит/с**) — упрощённая оценка «внешнего» трафика с пользовательских устройств. Часть отдается через CDN, есть внутренний трафик между микросервисами, базы данных и т.д., поэтому реальный суммарный network footprint у Booking.com выше.

3.3. RPS (Requests Per Second)

На 20 млн DAU и \~25,7 действий в день на 1 пользователя получаем:

25,7×20млн=514млн запросов/сутки≈ 514млн/86400≈5949RPS (в среднем)

**Разбивка по типам**:

| **Запрос**                 | **Суточное кол-во** | **Средний RPS** | **Пиковый RPS**    | **Комментарии**                                              |
|:---------------------------|---------------------:|----------------:|--------------------:|:-------------------------------------------------------------|
| **Поиск**                  | 200 млн             | ~2 300          | ~6900            | Списки отелей, фильтры и сортировки                          |
| **Просмотр отеля**         | 300 млн             | ~3 470          | ~12000            | Загрузка фото, отзывов, описаний                             |
| **Создание/изменение брони** | 2 млн              | ~23             | ~69               | Транзакции, оплата (наиболее «тяжёлые» для БД)               |
| **Отзывы**                 | 4 млн               | ~46             | ~120               | Чтение/написание отзывов                                     |
| **Сообщения (чат)**        | 4 млн               | ~46             | ~120               | Текстовые вопросы к отелю/службе поддержки                   |
| **Авторизация/Регистрация**| 2 млн               | ~23             | ~69               | Вход и создание новых учётных записей                        |
| **Управление объявлениями**| 2 млн               | ~23             | ~69               | Создание и редактирование объявлений                         |
| **Итого**                  | ~514 млн            | **~5 949**      | **~19000**        | Пиковые часы (вечер, праздники, акции)|


Ниже представлена доработанная версия с дополнительной таблицей, которая наглядно показывает ориентировочное распределение трафика и объясняет, почему для Booking.com оптимально именно **семь основных дата-центров** (при дальнейшем росте их можно расширять):

---

### Выбор локаций и распределение нагрузки для Booking.com

#### 1. Несколько дата-центров

- **Большое число пользователей по всему миру**  
  По данным SimilarWeb, на Booking.com ежемесячно заходит **более 500 млн** посетителей. Значимая доля трафика (до 40%) идёт из Европы, около 15–20% из Северной Америки, 15–25% из Азии и т.д.  
- **Снижение задержек и быстрая отдача**  
  При бронировании жилья пользователь просматривает десятки вариантов, перестраивает поисковые фильтры, грузит отзывы и фотографии. Даже лишние **2–3 секунды** ожидания могут повысить процент отказов и снизить конверсию в бронирования.  

#### 2. Таблица: распределение трафика и оправданность выбранных дата-центров

В таблице приводятся **примерные** доли по регионам (на основе открытых данных SimilarWeb, BusinessOfApps и внутренних оценок), основные страны, а также предлагаемая точка размещения одного или двух дата-центров на регион.

| **Регион**            | **Примерная доля трафика** | **Крупнейшие страны**                               | **Предлагаемые локации**                                          | **Основные аргументы**                                                                                                                                                                                        |
|-----------------------|:--------------------------:|----------------------------------------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Северная Америка**  | ~15–20%                   | США, Канада, Мексика                              | **Восток**: Нью-Йорк / Вирджиния - **Запад**: Лос-Анджелес / Сиэтл | - Высокая платёжеспособность и активность<br>- 1 DC на востоке, 1 на западе снижают задержки для всего континента - Широкие каналы связи с Европой и Азией                                                   |
| **Европа**            | ~35–40%                   | Великобритания, Германия, Франция, Италия, Испания, Нидерланды, Польша | **Амстердам** (историческая локация Booking) | - Самый крупный сегмент пользователей Нужна низкая латентность для быстрого поиска                   |
| **Азия**              | ~15–25%                   | Япония, Китай, Индия, Юго-Восточная Азия           | **Сингапур** (Дополнительно: Токио)    | - Огромное население и рост туризма Ускоренный доступ для пользователей из стран АТР - Актуально актив-актив репликация, чтобы обрабатывать бронирования локально                                      |
| **Латинская Америка** | ~10%                      | Бразилия, Мексика, Аргентина, Чили                | **Сан-Паулу** или **Мехико**                     | - Растущая доля рынка Ускоренное обслуживание стран Латинской Америки Уменьшение задержек при поиске и оплате бронирований                                                                           |
| **Африка**            | ~5%                       | Египет, ЮАР, Нигерия, Марокко                      | **Каир** (Север Африки) или **Йоханнесбург** (ЮАР) | - Пока небольшой, но перспективный рынок - Египет — одно из популярных туристических направлений (пляжный и культурный туризм) Локальный DC снижает задержки и даёт резерв для глобального покрытия      |

> **Итого**:  
> - **7 основных дата-центров**: США (2 шт), Европа (2 шт), Азия (1 или 2 шт), Латинская Америка (1), Африка (1).  
> - При дальнейшем росте можно добавлять дополнительные узлы или POP-точки.

#### 3. Распределение нагрузки: основные группы

Чтобы эффективно маршрутизировать миллионы запросов в секунду, Booking.com использует **GeoDNS** (определяет страну/регион) или Latency-based DNS (смотрит задержки) и отдаёт IP ближнего дата-центра, плюс **CDN c Anycast** для статики:

| **Группа нагрузки**                       | **Пример действий**                                  | **Технология / подход**                                | **Комментарий**                                                                                                                                |
|------------------------------------------|------------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| **Статический контент**                  | Фото отелей, миниатюры, стили, скрипты              | **CDN + Anycast**                                      | До 70% по объёму трафика (MB/GB). Быстро раздаётся из ближайшей POP-точки, разгружая основные сервисы                                          |
| **Поиск и просмотр**                     | Список объектов, фильтры, карточки с фото и отзывами| **GeoDNS → L4/L7 балансировка**                        | Самая массовая часть по количеству запросов (RPS), важна низкая задержка                                                                        |
| **Бронирования и оплата**                | Создание/изменение заказа, обработка платежа        | **Сервис с репликой БД**                | Критичная консистентность, требуют синхронную репликацию (Master-Master / Master-Standby)                                                       |
| **Отзывы, сообщения**                    | Чат, написание/чтение отзывов                       | **L7-балансировка**                   | Меньший объём, но постоянная активность (перед заездом, после выезда). Может работать частично асинхронно                                       |
| **Регистрация / авторизация**            | Вход, создание учётной записи                       | **Сервис аутентификации**                              |                                                                            |
| **Управление объявлениями** (host-кабинет)| Загрузка фото, редактирование описаний, календарь   | **L7-балансировка**            | Обычно нагружает системы хранения (при загрузке большого числа фотографий), но RPS относительно невысок                                         |

## Список источников

1. [Анализ веб-трафика booking.com](https://www.similarweb.com/ru/website/booking.com)

2. [Статистика Booking за 2023 год](https://www.businessofapps.com/data/booking-statistics/)

3. [https://ir.bookingholdings.com/financials/sec-filings/default.aspx](https://ir.bookingholdings.com/financials/sec-filings/default.aspx) 

4. [https://news.booking.com/](https://news.booking.com/)  
