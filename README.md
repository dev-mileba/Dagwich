# Dagwich ETL Project Template

Dagwich is a Copier template for generating a local Python ETL project with
Apache Airflow, PostgreSQL, and `psycopg2`. The generated project includes a
schema-driven CSV pipeline, Docker Compose services, and tests.

## Requirements

- Python 3.12
- [`uv`](https://docs.astral.sh/uv/)
- Docker with the Docker Compose plugin

## Generate a project

From the root of this template repository:

```sh
uv sync
uv run copier copy . ../generated-etl
```

Answer the prompts for the project/module names, Postgres table and columns,
and DAG settings. Defaults are provided. Copier creates the generated project
inside `../generated-etl/` using the selected project name. For example, with
the default name:

```sh
cd ../generated-etl/my_etl_project
```

## Run the generated ETL project

The generated project has its own `README.md` with the full setup and run
instructions. In brief:

```sh
cp .env.example .env
docker compose up --build -d --wait
```

Create the configured target table, run the generated DAG, and check its loaded
rows:

```sh
docker compose exec -T postgres sh -c \
  'psql -v ON_ERROR_STOP=1 -U "$POSTGRES_USER" -d "$POSTGRES_DB"' \
  < sql/create_target_table.sql
docker compose exec airflow airflow dags test my_etl_project_etl 2025-01-01
docker compose exec postgres sh -c \
  'psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -c "TABLE etl_records;"'
```

Replace the example DAG ID and table name with the values chosen in Copier.
Open <http://localhost:8080> for the Airflow UI. The generated project's
README documents tests, optional disposable-Postgres integration tests,
credentials, and how to stop services.

To stop the stack without deleting the database volume:

```sh
docker compose down
```
