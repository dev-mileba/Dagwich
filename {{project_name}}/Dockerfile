FROM apache/airflow:3.4.0-python3.12

COPY --chown=airflow:root pyproject.toml /tmp/etl-project/pyproject.toml
COPY --chown=airflow:root src /tmp/etl-project/src

RUN pip install --no-cache-dir \
    --constraint https://raw.githubusercontent.com/apache/airflow/constraints-3.4.0/constraints-3.12.txt \
    /tmp/etl-project
