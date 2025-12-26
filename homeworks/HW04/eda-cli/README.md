# S03 – eda_cli: мини-EDA для CSV

Небольшое CLI-приложение для базового анализа CSV-файлов.
Используется в рамках Семинара 03 курса «Инженерия ИИ».

## Требования

- Python 3.11+
- [uv](https://docs.astral.sh/uv/) установлен в систему

## Инициализация проекта

В корне проекта (S03):

```bash
uv sync
```

Эта команда:

- создаст виртуальное окружение `.venv`;
- установит зависимости из `pyproject.toml`;
- установит сам проект `eda-cli` в окружение.

## Запуск CLI

### Краткий обзор

```bash
uv run eda-cli overview data/example.csv
```

Параметры:

- `--sep` – разделитель (по умолчанию `,`);
- `--encoding` – кодировка (по умолчанию `utf-8`);
- `--title` – заголовок отчёта (по умолчанию `Отчёт`);
- `--max-hist-columns` – сколько числовых колонок включать в набор гистограмм.


### Полный EDA-отчёт

```bash
uv run eda-cli report data/example.csv --out-dir reports
```

В результате в каталоге `reports/` появятся:

- `report.md` – основной отчёт в Markdown;
- `summary.csv` – таблица по колонкам;
- `missing.csv` – пропуски по колонкам;
- `correlation.csv` – корреляционная матрица (если есть числовые признаки);
- `top_categories/*.csv` – top-k категорий по строковым признакам;
- `hist_*.png` – гистограммы числовых колонок;
- `missing_matrix.png` – визуализация пропусков;
- `correlation_heatmap.png` – тепловая карта корреляций.

## Тесты

```bash
uv run pytest -q
```

## Запуск HTTP-сервиса
```bash
uv run uvicorn eda_cli.api:app --reload --port 8000
```

Пояснения:

- `eda_cli.api:app` - путь до объекта FastAPI `app` в модуле `eda_cli.api`;
- `--reload` - автоматический перезапуск сервера при изменении кода (удобно для разработки);
- `--port 8000` - порт сервиса (можно поменять при необходимости).

После запуска сервис будет доступен по адресу:

```text
http://127.0.0.1:8000
```

## Эндпоинты сервиса

### 5. `POST /quality-flags-from-csv` – соблюдение флагов по CSV-файлу

Эндпоинт принимает CSV-файл, внутри:

- читает его в `pandas.DataFrame`;
- вызывает функции из `eda_cli.core`:

  - `summarize_dataset`,
  - `missing_table`,
  - `compute_quality_flags`;
- возвращает оценку качества датасета в том же формате, что `/quality`.

**Запрос:**

```http
POST /quality-flags-from-csv
Content-Type: multipart/form-data
file: <CSV-файл>
```

Через Swagger:

- в `/docs` открыть `POST /quality-flags-from-csv`,
- нажать `Try it out`,
- выбрать файл (например, `data/example.csv`),
- нажать `Execute`.

**Пример вызова через `curl` (Linux/macOS/WSL):**

```bash
curl -X POST "http://127.0.0.1:8000/quality-flags-from-csv" \
  -F "file=@data/example.csv"
```

Ответ будет содержать следующую структуру:

"flags": {
    "additionalProp1": true,
    "additionalProp2": true,
    "additionalProp3": true
}