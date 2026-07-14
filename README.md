# Hackathon-Practice-2026-Observers

Решение задачи регрессии для прогноза дохода клиента банка по табличным данным.

`DataAnalysis.ipynb` -- Исследовательский анализ исходных данных: target, веса, пропуски, корреляции, выбросы, категориальные признаки и различия train/test

`Preprocessing_and_FE.ipynb` -- Подготовка данных и feature engineering 

`notebooks/LGB_IMPROVED_ONLY.ipynb` -- Временная валидация, подбор конфигурации LightGBM, ансамбль, посткалибровка и создание submit

`submission_lgb_improved77158.csv` -- Выходной файл с предсказаниями, загруженный на платформу и получивший оценку 77158

`submission_lgb_improved76918.csv` -- Выходной файл с предсказаниями, загруженный на платформу и получивший оценку 76918

`requirements.txt` -- Закреплённые версии зависимосте


Используется weighted mean absolute error:

WMAE = sum_i (w_i) |y_i-y_pred_i| // sum_i (w_i)

Чем меньше WMAE, тем лучше решение. Веса `w` передаются в LightGBM как `sample_weight` и используются при локальном расчёте метрики.

## Pipeline

train.csv / test.csv

Preprocessing_and_FE.ipynb

train_processed_v2.csv / test_processed_v2.csv

LGB_IMPROVED_ONLY.ipynb

submission_lgb_improved.csv

`DataAnalysis.ipynb` является отдельным исследовательским notebook и не нужен для формирования submit.

## Быстрый запуск

1. Установить Python 3.11 и зависимости:
  bash
  python -m venv .venv
  source .venv/bin/activate
  python -m pip install --upgrade pip
  pip install -r requirements.txt
  jupyter lab

2. Открыть notebooks в порядке:

1. Preprocessing_and_FE.ipynb
2. LGB_IMPROVED_ONLY.ipynb

3. Перед выполнением каждого notebook перезапустить kernel и запускать ячейки сверху вниз.

## Входные данные

Исходные таблицы имеют формат CSV:

- разделитель: `;`;
- десятичный разделитель: `,`;
- train: 76 786 строк и 224 колонки;
- test: 73 214 строк и 222 колонки;
- только в train присутствуют `target` и `w`.


## Выходной файл

Модель создаёт: submission_lgb_improved.csv

Формат: id;predict

## Сохранённые локальные результаты

В выводах `LGB_IMPROVED_ONLY.ipynb` зафиксированы:

| Этап | Результат |
|---|---:|
| Лучшая отдельная конфигурация | `wide_127` |
| Selection WMAE лучшей конфигурации* | 60 133.0329 |
| Веса ансамбля | 0.5375 / 0.4625 |
| WMAE ансамбля на последнем фолде* | 60 145.0247 |*
| Финальная WMAE после посткалибровки* | 60 050.1431 |
| Масштаб калибровки | 1.02 |
| Сдвиг калибровки | −1 252.9316 |

*Это локальная временная валидация, а не leaderboard score.
