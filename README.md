# Прогнозирование временных рядов: исследование символьного и численного представления тиков

Курсовая работа, мехмат МГУ, кафедра вычислительной математики, 341 группа, 2026.
Журавлёв Г. В. Научный руководитель: д.ф.-м.н. М. И. Кумсков.

Текст работы: [`main.pdf`](main.pdf) (52 страницы).

## Главные результаты

- **UCR (морфологические ряды).** Три метода — ROCKET, MiniRocket, BOSS — образуют верхнюю группу со статистически неотличимым качеством (Friedman p = 1.8e-7) и значимо превосходят обучаемый Transformer на сыром численном сигнале. BOSS — лучший среди именно символьных методов.
- **BTC (финансовый ряд).** Ни одна из 12 проверенных современных архитектур (DLinear, NLinear, NHITS, NBEATSx, PatchTST, TimesNet, TFT, TiDE, iTransformer, FuzzyLSTM, Chronos Bolt Small, символьный Transformer) не превосходит тривиальный `Naive`-прогноз ни на одной из проверенных конфигураций (Wilcoxon p = 0.008, win/loss 0/8 для каждой архитектуры).
- **Backtest.** Hit rate 0.53–0.54 не покрывает транзакционных издержек.

## Структура репозитория

```
.
├── main.pdf                           # текст курсовой
├── notebooks/
│   ├── btc/                           # 5 ноутбуков для финансового домена
│   └── ucr/                           # 4 ноутбука для UCR
├── results/                           # CSV-выгрузки confirmatory-прогонов
├── data/                              # исходные ценовые ряды BTC
├── figures/                           # PNG для рисунков работы
└── requirements.txt                   # пиннинг ключевых библиотек
```

## Соответствие ноутбуков таблицам/рисункам в работе

### Финансовый домен (`notebooks/btc/`)

| Ноутбук | Воспроизводит |
|---|---|
| `btc_transformer_confirmatory_study.ipynb` | Табл. 5 (BTC регрессия), Табл. 6 (BTC направление), Табл. 14 (direction сводно) |
| `btc_onestep_neuralforecast.ipynb` | Табл. 12 (BTC SOTA one-step) |
| `btc_walkforward_sota.ipynb` | Табл. 12 (walk-forward подтверждение) |
| `btc_eth_multistep_horizons.ipynb` | Табл. 12 + 13 (BTC и ETH multi-step), Wilcoxon |
| `btc_direction_backtest.ipynb` | Табл. 16 + рис. 5 (Sharpe, max drawdown, hit rate) |

### UCR (`notebooks/ucr/`)

| Ноутбук | Воспроизводит |
|---|---|
| `symbolic_vs_numeric_coursework.ipynb` | Табл. 7 + рис. 1 (UCR small), Табл. 9 + рис. 3 (шумовой sweep) |
| `ucr_rocket_minirocket.ipynb` | Табл. 7 (ROCKET/MiniRocket на small), Табл. 18 (вычислительная сложность) |
| `fair_comparison.ipynb` | Табл. 8 + рис. 2 (PatchTST с аугментацией), Табл. 10 + рис. 4 (UCR large) |
| `ucr_rocket_only_large.ipynb` | Табл. 10 (ROCKET/MiniRocket на large), Табл. 18 (сложность) |

## Среда выполнения

Все эксперименты запускались на **Kaggle (T4)** или **Google Colab** с Python 3.11/3.12. Локальное воспроизведение возможно при наличии CUDA-совместимого GPU (≥ 8 GB).

**Важно:** Kaggle P100 (sm_60) несовместим со свежими сборками PyTorch — при запуске на Kaggle переключите Accelerator на T4 (sm_75).

### Установка зависимостей

```bash
pip install -r requirements.txt
```

Ключевой пиннинг (см. также `requirements.txt`):
```
numpy<2.2 pandas<2.4
neuralforecast==1.7.5      # ставить с флагом --no-deps (иначе тянет несовместимые версии)
```

### Установка neuralforecast без зависимостей

```bash
pip install --no-deps neuralforecast==1.7.5
pip install ray utilsforecast coreforecast  # ручная установка нужных рантайм-зависимостей
```

## Данные

| Файл | Источник |
|---|---|
| `data/data_btc_1h.csv` | BTC/USDT часовая частота, Binance (через `python-binance`) |
| `data/COINBASE_BTC_1m_last120d.csv` | BTC/USD 1-минутная частота, Coinbase (последние 120 дней) |

UCR-датасеты автоматически скачиваются ноутбуками через `aeon` (`load_classification('Coffee')`, и т. п.). Локальная копия архива не требуется.

## Лицензия

Содержимое (текст работы, ноутбуки) — для академического использования с указанием авторства.
