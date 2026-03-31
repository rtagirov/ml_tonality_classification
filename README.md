## Классификация тональности текста (english version below)

### Описание проекта

Цель проекта — построить модель машинного обучения для классификации тональности текста на три класса:

- позитив
- нейтрал
- негатив

Задача включает:
- предобработку данных
- обучение модели
- оценку качества
- анализ ошибок
- предложение улучшений

Основной акцент — не только на точности, но и на понимании поведения модели, особенно при дисбалансе классов.

---

### Датасет

Датасет состоит из:

- text — входной текст (пользовательский контент)
- sentiment — целевая переменная (3 класса)

Ключевые наблюдения:
- небольшой датасет (~3000 наблюдений)
- умеренный дисбаланс классов:
  - позитив ~45%
  - нейтрал ~37%
  - негатив ~17%
- несогласованность в названиях классов (например, "позит", "позитивный") — нормализованы в процессе предобработки

---

### Подход

#### 1. Предобработка данных
- удалены пропущенные значения
- нормализованы метки классов
- выполнена базовая очистка текста

#### 2. Разбиение на train / validation
- разбиение 80 / 20
- стратификация по таргету
- random_state = 42

#### 3. Базовая модель
- TF-IDF векторизация
- Logistic Regression

Результаты:
- Accuracy ≈ 0.70
- F1 (weighted) ≈ 0.67
- F1 (macro) ≈ 0.59

---

### Ключевая проблема

Базовая модель показывает:

- очень низкий recall для класса "негатив"
- смещение в сторону доминирующих классов

---

### Эксперименты

#### 1. n-grams

- Добавлены биграммы (ngram_range=(1,2))

Результат:
- улучшения метрик не произошло
- структура ошибок не изменилась

> Вывод: представление признаков не является основным ограничением.

#### 2. class_weight='balanced'

- Применена балансировка классов в Logistic Regression

Результат:
- Accuracy немного снизилась
- F1 (weighted) вырос
- F1 (macro) вырос значительно

---

### Ключевые выводы

#### Дисбаланс классов — основная проблема

- базовая модель игнорирует минорный класс
- сбалансированная модель лучше распознаёт негативные примеры

#### Trade-off

| Метрика | Эффект |
|------|--------|
| Accuracy | немного хуже |
| Recall (negative) | значительно лучше |
| Precision (negative) | снизился |
| F1 macro | значительно улучшился |

#### Интерпретация

Модель переходит от:

- консервативной стратегии (избегает предсказания негатива)

к:

- агрессивной стратегии (активно выявляет негативный класс)

---

### Переобучение

Все модели демонстрируют:

- значительный разрыв между train и validation

> Переобучение присутствует и не устраняется ни за счёт признаков, ни за счёт балансировки классов.

---

### Итоговое заключение

- основное ограничение — дисбаланс классов, а не признаки
- n-grams не улучшают качество
- балансировка классов улучшает равномерность качества по классам
- наблюдается явный trade-off между:
  - общей точностью
  - качеством по классам

---

### Возможные улучшения

- подбор гиперпараметров (регуляризация)
- более продвинутые модели (например, linear SVM)
- улучшенная предобработка текста (лемматизация, стоп-слова)
- настройка порогов классификации
- увеличение объёма данных

---

### Итог

Финальная модель даёт более сбалансированное и осмысленное решение, несмотря на небольшое снижение accuracy.

> Для данной задачи F1 (особенно macro) является более информативной метрикой, чем accuracy.

## Text Sentiment Classification

### Overview

The goal of this project is to build a machine learning model for text sentiment classification into three classes:

- positive
- neutral
- negative

The task includes:
- data preprocessing
- model training
- evaluation
- error analysis
- and proposing improvements

The main focus is not only on achieving high accuracy, but on understanding model behavior, especially on imbalanced data.

---

### Dataset

The dataset consists of:

- text — input text (user-generated content)
- sentiment — target label (3 classes)

Key observations:
- small dataset (~3000 samples)
- moderate class imbalance:
  - positive ~45%
  - neutral ~37%
  - negative ~17%
- inconsistent labels (e.g. "posit", "positive") — normalized during preprocessing

---

### Approach

#### 1. Data preprocessing
- removed missing values
- normalized sentiment labels
- basic text cleaning

#### 2. Train / Validation split
- 80 / 20 split
- stratified sampling
- random_state = 42

#### 3. Baseline model
- TF-IDF vectorization
- Logistic Regression

Results:
- Accuracy ≈ 0.70
- F1 (weighted) ≈ 0.67
- F1 (macro) ≈ 0.59

---

### Key issue

The baseline model shows:

- very low recall for the "negative" class
- bias toward majority classes

---

### Experiments

#### 1. n-grams

- Added bigrams (ngram_range=(1,2))

Result:
- no improvement
- no change in error structure

> Conclusion: feature representation is not the main limitation.

#### 2. class_weight='balanced'

- Applied class balancing in Logistic Regression

Result:
- Accuracy decreased slightly
- F1 (weighted) increased
- F1 (macro) increased significantly

---

### Key Findings

#### Class imbalance is the main issue

- the baseline model ignores the minority class
- the balanced model improves detection of negative samples

#### Trade-off

| Metric | Effect |
|------|--------|
| Accuracy | slightly worse |
| Recall (negative) | strongly improved |
| Precision (negative) | decreased |
| F1 macro | significantly improved |

#### Interpretation

The model shifts from:

- conservative strategy (avoiding negative predictions)

to:

- aggressive strategy (actively detecting the negative class)

---

### Overfitting

All models show:

- significant train-validation gap

> Overfitting is present and is not resolved by either feature engineering or class balancing.

---

### Final Conclusion

- the main limitation is class imbalance, not feature representation
- n-grams do not improve performance
- class balancing improves fairness across classes
- there is a clear trade-off between:
  - overall accuracy
  - class-level performance

---

### Possible Improvements

- hyperparameter tuning (regularization)
- more advanced models (e.g. linear SVM)
- better text preprocessing (lemmatization, stopwords)
- threshold tuning
- more data

---

### Summary

The final model provides a more balanced and meaningful solution, even with a small drop in accuracy.

> In this task, F1 (especially macro) is more informative than accuracy.
