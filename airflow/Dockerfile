FROM python:3.8.2-slim

ARG OWNER=airflow

ENV PYTHONDONTWRITEBYTECODE=1
ENV AIRFLOW_HOME=/opt/airflow
ENV AIRFLOW_LOG_DIR=/var/log/airflow
ENV AIRFLOW_DB_URL=sqlite:////tmp/db.sqlite.airflow

WORKDIR ${AIRFLOW_HOME}

COPY . .

RUN set -x \
 && groupadd -g 1000 ${OWNER} \
 && useradd -u 1000 -g 1000 -m -s /bin/bash ${OWNER} \
 && mkdir -p ${AIRFLOW_LOG_DIR} \
 && chown -R ${OWNER}:${OWNER} ${AIRFLOW_LOG_DIR} \
 && apt-get update -yqq \
 && apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
 && pip install -U pip setuptools poetry \
 && poetry config virtualenvs.create false \
 && poetry install --no-root \
 && apt-get -y clean \
 && rm -rf /var/lib/apt/lists/*

USER ${OWNER}
EXPOSE 8080