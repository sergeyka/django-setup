
# https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/

# pull official base image
FROM python:3.9.6-alpine

# set work directory
WORKDIR /usr/src/app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apk update \
    && apk add curl postgresql-dev gcc python3-dev musl-dev

# install dependencies
RUN pip install --upgrade pip
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
ENV PATH "/root/.local/bin:$PATH"
RUN source $HOME/.poetry/env


COPY ./poetry.lock ./pyproject.toml ./
RUN $HOME/.poetry/bin/poetry config virtualenvs.create false \
    && $HOME/.poetry/bin/poetry install --no-dev --no-interaction --no-ansi

RUN $HOME/.poetry/bin/poetry install
# copy project
COPY . .

ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
