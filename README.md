# Shift Yourself Left: Integration Testing for Data Engineers

This repository contains the code for the Shift Yourself Left: Integration Testing for Data Engineers project.
It is intended to be the simplest possible example of how to integrate a containerized data pipeline built
using [DuckDB](https://duckdb.org/), [dbt-duckdb](https://github.com/duckdb/dbt-duckdb), and [dlt](https://dlthub.com/)
with an upstream web service built using [FastAPI](https://fastapi.tiangolo.com/) and MySQL such that the
application and its data pipeline can be containerized, deployed, and tested as part of a CI/CD system. This
enables the data pipeline to be created and tested in a way that is fully integrated with its upstream
web service and allows us to test as much of the pipeline as possible in a real-world environment *before* the
integration is deployed to production.

The project is split into three parts:

1. The FastAPI web service, which provides a simple API for managing recipes. This is defined in the `app` directory.
1. The data pipeline, which is defined in the `pipeline` directory and is a standard dbt-duckdb project that uses
dlt to load data from an upstream database into a local DuckDB instance.
1. The integration testing project, which is defined in the `integration-testing` directory and contains the
[Docker Compose](https://docs.docker.com/compose/) configuration for running the web service and data pipeline together.
  * The `app/tests` directory contains a [pytest](https://docs.pytest.org/) test suite that exercises the web service API.
  * The `pipeline/run_all.sh` runs the data ingestion with dlt and then the data transformation with dbt.
  * The `integration_tests/run.sh` script integrates the two:
    1. It first runs the `pytest` suite which populates the `app` database with some test fixture data.
    1. It then triggers the `pipeline` and will fail if this doesn't complete successfully.

The simplest way to get started is to execute the `run.sh` script in the `integration_tests` directory, which will
build the Docker images for the web service and data pipeline, run them in Docker Compose, and execute the test
suite and pipeline.

## Quickstart

    # Clone the repo
    git clone https://github.com/jwills/shift_yourself_left.git

and

    # Run the integration tests
    cd integration_tests && ./run.sh
