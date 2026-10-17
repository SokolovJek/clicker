Сервис для сокращения ссылок на **FastAPI + SQLAlchemy (async) + PostgreSQL**, с простым фронтендом (HTML/JS) и запуском через **Docker Compose**.

---

## 📦 Стек

- **Python 3.12**
- **FastAPI** + Pydantic
- **SQLAlchemy 2.x** (async) + **asyncpg**
- **PostgreSQL**
- **Pytest** (в т.ч. `pytest-asyncio`)
- **Docker Compose**

---

## 🚀 Быстрый запуск через Docker Compose

1. Убедитесь, что установлены **Docker** и **Docker Compose**.
2. Клонируйте репозиторий и перейдите в корень проекта.

3. Соберите и запустите:

   ```bash
   docker compose up --build
   ```

4. Приложение будет доступно по адресу:

   - API: `http://localhost:8000`
   - Swagger UI: `http://localhost:8000/docs`
   - Redoc: `http://localhost:8000/redoc`

5. Откройте `index.html` в браузере (файл фронтенда), либо разместите его на любом статичном сервере.
   По умолчанию фронтенд обращается к `http://localhost:8000`.

---

## 🐍 Локальный запуск без Docker

### 1. Требования

- Python **3.12**
- PostgreSQL (локально или в Docker)

### 2. Установка зависимостей

Рекомендуется использовать `uv` (в проекте есть `uv.lock`):

```bash
uv sync
```

Или через `pip` (зависимости перечислены в `pyproject.toml`/`requirements`):

```bash
python -m venv .venv
source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -e .
```

### 3. Настройка БД

Создайте базу данных PostgreSQL, например:

```sql
CREATE DATABASE shortener;
```


### 4. Запуск приложения

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

---

## 🧪 Тесты

```bash
pytest -v
```

---

## 🔌 API

### `POST /short_url`

Сократить ссылку.

**Request:**

```json
{
  "long_url": "https://example.com/some/very/long/path"
}
```

**Response:**

```json
{
  "data": "abc123"
}
```

Где `data` — сгенерированный slug.

### `GET /{slug}`

Редирект на исходный длинный URL.

---

## 🖥️ Фронтенд

Файл `index.html` — простая форма:

- поле для длинного URL;
- кнопка «Сократить»;
- отображение короткой ссылки.

Для работы фронтенда backend должен быть доступен по адресу, указанному в переменной:

```js
const backendBase = "http://localhost:8000";
```

---

## 📁 Структура проекта (примерная)

```
.
├── docker-compose.yml
├── pyproject.toml
├── uv.lock
├── index.html
├── srs/
│   └── ...
└── tests/
    └── ...
```

---

## 📝 Примечания

- Проект поддерживает **Python >= 3.9**, однако целевая версия — **3.12**.
- Для асинхронной работы с PostgreSQL используется **asyncpg**, для SQLite — **aiosqlite** (полезно в тестах).
- Тесты используют `pytest-asyncio` и `httpx` (для тестирования FastAPI-эндпоинтов).
- В `uv.lock` зафиксированы версии зависимостей — используйте `uv sync` для воспроизводимой установки.