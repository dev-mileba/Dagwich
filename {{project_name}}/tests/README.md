# Tests

Run the unit tests from the project root:

```sh
uv sync --extra dev
uv run pytest -q
```

The PostgreSQL integration test is skipped unless `ETL_TEST_DATABASE_URL` is
set. Start the isolated, in-memory test database and run the test with:

```sh
docker compose --profile test up -d --wait postgres-test
ETL_TEST_DATABASE_URL=postgresql://etl_test_user:local_test_only@127.0.0.1:5433/etl_test_db \
  uv run pytest -q -m integration
docker compose --profile test down
```

The test database uses a separate PostgreSQL service backed by temporary
container storage, not the regular ETL database. The test creates a uniquely
named table and drops it when complete.
