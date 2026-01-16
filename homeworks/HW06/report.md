# HW06 – Report

> Файл: `homeworks/HW06/report.md`  
> Важно: не меняйте названия разделов (заголовков). Заполняйте текстом и/или вставляйте результаты.

## 1. Dataset

- Какой датасет выбран: `S06-hw-dataset-01.csv`
- Размер: 12000 строк; 30 столбцов
- Целевая переменная: `target` - бинарный классификация: 0 - 67%, 1 - 33%
- Признаки: float64 (num01 - num24), int64 (остальные признаки)

## 2. Protocol

- Разбиение: train/test (75%/25%, `random_state`)
- Подбор: CV на train (сколько фолдов, что оптимизировали)
- Метрики: accuracy, F1, ROC-AUC (и почему эти метрики уместны именно здесь)

## 3. Models

Опишите, какие модели сравнивали и какие гиперпараметры подбирали.

Минимум:

- DummyClassifier (baseline)
- LogisticRegression (baseline из S05)
- DecisionTreeClassifier (контроль сложности: `max_depth` + `min_samples_leaf` или `ccp_alpha`)
- RandomForestClassifier
- HistGradientBoosting


## 4. Results

- Таблица/список финальных метрик на test по всем моделям
![График рейтинга моделей](artifacts/figures/rating.png)
- Победитель по всем критериям стала модель HistGradientBoosting. (Краткое описание)

## 5. Analysis

- Устойчивость: при изменении `random_state` на модели HistGradientBoosting было устоновленно, что значения метрик качества меняются в порядке тысячных.
- Ошибки: confusion matrix для лучшей модели + комментарий
<br>
<img src="artifacts/figures/confusion matrix.png" alt="Confusion Matrix" width="300">
- Интерпретация: permutation importance (top-10/15) + выводы
<br>
<img src="artifacts/figures/permutation importance (top-15).png" alt="permutation importance (top-15)" width="500">

## 6. Conclusion

3-6 коротких тезисов: что вы поняли про деревья/ансамбли и про честный ML-протокол.
