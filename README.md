# RLinear и RevIN

Репозиторий для воспроизведения статьи *Revisiting Long-term Time Series Forecasting: An Investigation on Affine Mapping*. В проекте мы проверяем, действительно ли простая линейная модель с RevIN работает на стандартных временных рядах почти так же хорошо, как PatchTST и TimesNet, и насколько итоговые числа зависят от протокола оценки.

Сейчас готов авторский пилот RLinear на ETTh1 и несколько дополнительных проверок этой же модели. Основные запуски пяти архитектур еще не проводились. Полный план и ожидания записаны в [`PRE_REGISTRATION.md`](PRE_REGISTRATION.md).

Проект выполняется в рамках [курса Deep Learning](https://github.com/xufana/dl-knad-course/blob/main/project.md). Решения по экспериментам записываем в [`DECISIONS.md`](DECISIONS.md).

## Первые результаты

Авторский пилот RLinear получил MSE **0.3660** и MAE **0.3909**, что совпадает с округлёнными числами статьи. В дополнительных проверках на трёх сидах RevIN и более длинная история не улучшили MSE RLinear. Отбрасывание последних тестовых окон снизило ошибку примерно на 1.24%. Подробности и таблица — в [`results/README.md`](results/README.md).

## Структура

- `pilot/`: stdout исходной и исправленной команд авторов;
- `results/`: первые контролируемые проверки RLinear;
- `scripts/run_pilot.sh`: загрузка зафиксированных версий кода и данных и запуск пилота;
- `scripts/check_rlinear.py`: проверки RevIN, `drop_last`, длины истории и сдвига уровня;
- `requirements-pilot.txt`: окружение, в котором выполнялся пилот.

## Как повторить пилот

Для запуска нужен Python 3.11:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements-pilot.txt
```

Исходная команда из `script.md` авторов падает с `KeyError: 'Linear'`. Это поведение можно повторить так:

```bash
bash scripts/run_pilot.sh original
```

Исправленный запуск использует зарегистрированное имя `RLinear`:

```bash
bash scripts/run_pilot.sh rlinear
```

Дополнительные проверки на трёх сидах запускаются отдельно:

```bash
bash scripts/run_pilot.sh controlled
```

Логи новых запусков и `results.json` контролируемой проверки сохраняются в отдельной папке `.work/runs/`; путь печатается скриптом. Замеров на T4 пока нет, локальные 24.1 секунды относятся только к авторскому пилоту. Команд для полной таблицы пяти архитектур пока нет: это следующий этап проекта.
