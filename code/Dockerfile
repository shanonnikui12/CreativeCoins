FROM node:21-alpine AS frontend

WORKDIR /frontend

COPY ./frontend/react-app/package.json ./frontend/react-app/package-lock.json ./

RUN yarn install

COPY ./frontend/ ./

# ポート3000を開放
EXPOSE 3000

# アプリケーションを起動
CMD ["yarn", "start"]


FROM python:3.12.3-slim AS backend_base

WORKDIR /backend_env

RUN pip install poetry==1.6.1

ENV POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

WORKDIR /backend_env

COPY ./backend/pyproject.toml ./backend/poetry.lock ./
RUN poetry install && rm -rf $POETRY_CACHE_DIR


FROM python:3.12.3-slim AS backend

RUN apt-get update && apt-get -y upgrade && apt-get install -y ffmpeg

ENV VIRTUAL_ENV=/backend_env/.venv \
    PATH="/backend_env/.venv/bin:$PATH"

WORKDIR /backend
COPY --from=backend_base /backend_env/.venv /backend_env/.venv
COPY backend/ ./
COPY CHANGELOG.md ./CHANGELOG.md

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
