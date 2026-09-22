# Retail KPI Planning & Optimization Model (2025)

An automated data-driven target-setting and optimization engine that models and distributes corporate retail KPIs (Conversion Rate and Units Per Transaction) across a commercial network.

## Choose Your Language / Выберите язык
* [English](#english-doc)
* [Русский](#russian-doc)

---

<a id="english-doc"></a>
## English Documentation

### Project Overview
Traditional retail target-setting ("flat targeting" or flat percentage increases) frequently demotivates personnel in historically weaker locations and leaks margin in inherently dominant ones. This project addresses this operational mismatch by engineering a hybrid **Bottom-Up profiling** and **Top-Down calibration** predictive model for a commercial network of 502 stores. 

Using raw transactional historical data, this automated Python engine generates localized, mathematically sound, and aggregated targets for the peak trading season of May–August 2025 across two core retail metrics: **Conversion Rate (CR)** and **Units Per Transaction (UPT)**.

### Repository Structure
* `01_KPI_Planning_Model.ipynb` — Core Jupyter Notebook containing the end-to-end data pipeline, Exploratory Data Analysis (EDA), metric engineering, stability analysis, and the dynamic calibration model.
* `Тестовое задание_аналитик_2025.xlsx` — Source dataset including daily transaction logs (111,258 rows) and corporate macro-targets.

### Key Methodology & Mathematical Architecture

#### 1. Exploratory Data Analysis & Sanitization
* Identified and removed technical anomalies (e.g., footfall = 0 with transactions > 0, buyers > visitors, items sold < buyers).
* Isolated April 2025 from the seasonal baseline calculation because it contained partial logs (days 1–17 only). It was correctly shifted into a near-term operational momentum tracker.

#### 2. Stability & Autocorrelation Analysis
Individual store performance was isolated from network fluctuations by indexing metrics against the aggregate corporate average for each specific month:
$$Index = \frac{\text{Store Monthly Metric}}{\text{Company Average for the Same Month}}$$
A Pearson correlation matrix mapped between 2024 and 2025 verified historical stability:
* **UPT Index (r = 0.9033):** Exceptionally stable. Proves that transaction depth is an internal operational metric controlled by staff execution, cross-selling protocols, and merchandising.
* **Conversion Index (r = 0.7771):** Moderately high stability. More volatile as it is structurally exposed to external vectors (marketing, local footfall quality, and weather).

#### 3. Execution Pipeline & Top-Down Calibration
1. **Dynamic Target Ingestion:** Corporate high-level goals are dynamically read from the source data frame into a python dictionary using the `.to_dict(orient="index")` pipeline.
2. **Matrix Expansion:** The script creates a long-form structural backbone for every unique `Store ID` × `Forecast Month (May–August)`.
3. **Initial Plan Projection:**
$$InitialPlanConv = \text{TargetConv} \times \text{ConvIndex}$$
$$InitialPlanUPT = \text{TargetUPT} \times \text{UPTIndex}$$
4. **Convergence Audit & Calibration:** 
Simple multiplication doesn't converge network-wide due to structural weight shifts (e.g., high-conversion boutiques vs. massive footfall megastores). To resolve this without destroying store-level variance, a dynamic monthly scaling vector (`Calib_Factor`) is calculated:
$$CalibFactor = \frac{CorporateTarget}{InitialPlanMean}$$
5. **Final Matrix Scaling:**
$$FinalPlan = InitialPlan \times CalibFactor$$

### Core Business Insights for Leadership
* **UPT Goals are Conservative:** The computed `Calib_Factor` consistently stabilised above parity (1.04 to 1.07). This indicates that corporate UPT expectations are slightly lower than the real historical capacity of the stores, representing a highly viable path for retail teams to beat targets.
* **Conversion Goals are Aggressive:** Calibration factors required a downward contraction (multiplying by 0.89 for August). The corporate office set targets without factoring in late-summer footfall drops. The model successfully balanced the targets to make them achievable while maintaining competitive distribution.

---

<a id="russian-doc"></a>
## Русскоязычная документация

### Описание проекта
Традиционный подход к планированию («уравниловка» или пропорциональное увеличение от факта прошлого года) приводит к демотивации персонала в исторически слабых локациях и недополучению прибыли в сильных. Данный проект решает эту задачу с помощью построения прогнозной модели, сочетающей **индивидуальное профилирование (Bottom-Up)** и **макро-калибровку (Top-Down)** для розничной сети из 502 магазинов.

Используя сырые исторические данные продаж, автоматизированный скрипт на Python генерирует локализованные, математически выверенные и агрегированные планы на пиковый сезон Май–Август 2025 года по двум ключевым метрикам ритейла: **Конверсии (CR)** и **Среднему количеству товаров в чеке (UPT)**.

### Структура репозитория
* `01_KPI_Planning_Model.ipynb` — Основной Jupyter Notebook, содержащий полный цикл обработки данных, разведочный анализ (EDA), расчет индексов, анализ стабильности временных рядов и финальную калибровочную модель.
* `Тестовое задание_аналитик_2025.xlsx` — Исходный датасет с ежедневными логами продаж (111 258 строк) и макро-целями компании.

### Методология и математическая архитектура

#### 1. Разведочный анализ (EDA) и очистка данных
* Обнаружены и удалены технические аномалии (например, посетители = 0 при покупателях > 0, покупатели > посетителей, штуки < покупателей).
* Апрель 2025 года был исключен из расчета сезонного профиля, так как содержал данные только за 1–17 числа. Он был корректно переведен в статус индикатора краткосрочного оперативного тренда.

#### 2. Анализ стабильности и автокорреляции
Сила каждого магазина была изолирована от сетевых колебаний путем расчета индивидуальных индексов относительно среднего значения компании за конкретный месяц:
$$Index = \frac{\text{Метрика конкретного магазина за месяц}}{\text{Средняя метрика компании за этот же месяц}}$$
Матрица корреляции Пирсона между аналогичными периодами 2024 и 2025 годов подтвердила высокую устойчивость профилей:
* **Индекс UPT (r = 0.9033):** Высочайшая линейная связь. Доказывает, что глубина чека — внутренний операционный показатель, определяемый стандартами обслуживания и качеством работы персонала в зале.
* **Индекс Конверсии (r = 0.7771):** Умеренно-высокая связь. Метрика более волатильна, так как напрямую зависит от внешних факторов (маркетинг конкурентов, качество входящего трафика, погода).

#### 3. Алгоритм расчета и Top-Down калибровка
1. **Динамический импорт таргетов:** Из фрейма `df_goals` цели компании автоматически переводятся в рабочий словарь с помощью метода `.to_dict(orient="index")`, что исключает жестко вбитые параметры (хардкод).
2. **Развертывание матрицы:** Генерируется плоская таблица, содержащая уникальные комбинации `ID магазина` × `Прогнозный месяц (Май–Август)`.
3. **Расчет первоначального плана (Initial Plan):**
$$InitialPlanConv = \text{TargetConv} \times \text{ConvIndex}$$
$$InitialPlanUPT = \text{TargetUPT} \times \text{UPTIndex}$$
1. **Контроль сходимости (Этап Check):**
Математически, простое перемножение индексов на уровне Bottom-Up не сходится с планом корпоративного центра из-за структурных сдвигов и разного масштаба магазинов. Для устранения этого дисбаланса модель рассчитывает помесячный коэффициент калибровки:
$$CalibFactor = \frac{CorporateTarget}{InitialPlanMean}$$
1. **Финальный расчет (Final Plan):**
$$FinalPlan = InitialPlan \times CalibFactor$$

### Ключевые выводы для руководства
* **Цели по UPT — консервативные:** Коэффициенты калибровки по UPT стабильно зафиксировались выше единицы (`1.04 - 1.07`). Это показывает, что централизованный план компании изначально заложен с небольшим запасом прочности относительно реального потенциала магазинов. Это создает для розницы отличные условия для перевыполнения планов.
* **Цели по Конверсии — агрессивные:** Модель потребовала снижения планов на август (умножением на коэффициент `0.89`). Корпоративный центр выставил одинаковые цели без учета естественного спада трафика в конце лета. Модель успешно скорректировала этот перекос, сделав планы выполнимыми.

---

### Requirements / Требования
To run the code, ensure you have the following libraries installed / Для запуска кода убедитесь, что установлены следующие библиотеки:
```bash
pip install pandas numpy openpyxl matplotlib seaborn
```