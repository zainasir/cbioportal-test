# cBioPortal Tests
This repo hosts the test-runner scripts that are used throughout cbioportal applications. The tests themselves vary from repo to repo, and are contained within the source code (e.g. cbioportal-frontend hosts specs for the frontend).

## Setup
Before running any of the scripts, make sure to export the required environment variables shown below. If you need access to default values, get in touch with [Zain](mailto:nasirz1@mskcc.org).
```shell
set -o allexport
export DOCKER_USERNAME=<docker-username>
export DOCKER_PASSWORD=<docker-password>
export DOCKER_IMAGE_CBIOPORTAL=<docker-compose-image-tag>
export DOCKER_TAG=<build-tag>
export DB_MYSQL_USERNAME=<mysql-username>
export DB_MYSQL_PASSWORD=<mysql-password>
export DB_MYSQL_URL=<mysql-url>
export DB_CLICKHOUSE_USERNAME=<clickhouse-username>
export DB_CLICKHOUSE_PASSWORD=<clickhouse-password>
export DB_CLICKHOUSE_URL=<clickhouse-url>
set +o allexport
```

## Usage
All [scripts](./scripts) are standalone and can be run independently. They can also be configured by setting the appropriate environment variables and flags.
> NOTE: Call scripts from the root directory. If called from any other subdirectory, the relative paths used by scripts will not work.

### [build-push-image.sh](./scripts/build-push-image.sh)
Build a docker image using the source code provided.

#### Args:
- _--src=/path/to/dockerfile_ (REQUIRED)
- _--push=false_ (If _true_, push to cbioportal/cbioportal-dev on dockerhub)
- _--skip_web=false_ (If _true_, skip building the web-shenandoah image)
- _--skip_web_and_data=false_ (If _true_, skip building the web-and-data image)

```shell
# To also push image to DockerHub, set --push=true. Default is false.
sh ./scripts/docker-compose.sh --src=/path/to/dockerfile --push=false
```

### [docker-compose.sh](./scripts/docker-compose.sh)
Start a cbioportal instance at localhost:8080. Set the appropriate environment variables to configure the database and docker image to use.

#### Args:
- _--portal_type=web_ (If _--portal_type=web-and-data_, launch the web-and-data image.)
- _--docker_args_ (Optional docker compose args, e.g. '_--detach --build_')

```shell
sh ./scripts/docker-compose.sh --portal_type=web
```

### [import-data.sh](./scripts/import-data.sh)
Import data into a cbioportal instance running at localhost:8080.

#### Args:
- _--study_list=/path/to/study_list.txt_ (REQUIRED. File containing whitespace-delimited list of studies to import)

```shell
sh ./scripts/import-data.sh --study_list=/path/to/study_list.txt
```

## Troubleshoots
If docker compose gets stuck at "_Database not available yet (first time can take a few minutes to load seed database)... Attempting reconnect..._", try pruning the docker system and rerun script:
```shell
docker system prune -a
./scripts/docker-compose.sh
```
