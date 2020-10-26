
<p align="center">
  <a target="_blank" rel="noopener noreferrer">
    <img width="75%" src="public/docker-compose-metabase.png" alt="Docker+Compose+Metabase" />
  </a>
</p>

<h4 align="center">This project intends to facilitate the task of developing a production ready environment for <a href="https://www.metabase.com/docs/" target="_blank">Metabase</a>.</h4>

<p align="center">
  <a href="https://github.com/Cambalab/metabase-compose/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-GPL--3.0-orange" alt="License"></a>
  <a href="https://github.com/Cambalab/metabase-compose/blob/main/.github/CODE_OF_CONDUCT.md"><img src="https://img.shields.io/badge/Contributor%20Covenant-v2.0%20adopted-ff69b4.svg" alt="License"></a>
</p>

# Metabase Compose

## About

This document does not explain how to use Metabase, please refer to the official documentation for usage instructions.

The Compose uses the following images:

+ [Metabase](https://hub.docker.com/r/metabase/metabase)
+ [Postgres](https://hub.docker.com/_/postgres)
+ [MySQL](https://hub.docker.com/_/mysql)
+ [Adminer](https://hub.docker.com/_/adminer)

> The database images (`mysql`, `postgres`) are created via dockerfiles that a wrap the official images.

## Development

### Requirements

+ [Docker](https://docs.docker.com/)
+ [Docker Compose](https://docs.docker.com/compose/#compose-documentation)


### Configuration

Container configurations depend on environment variables defined in an `.env` file.

+ Copy the `.env.example` file and rename it to `.env`
+ Replace the environment values to your desire. See environment explanatory chart below

### Environment variables

Database environment:

+ `DB_TYPE:` can be either `postgres` or `mysql`
+ `DB_NAME:` the name of the database
+ `DB_USER:` a database user different than root
+ `DB_PASSWORD:` a password for `DB_USER`
+ `DB_ROOT_PASSWORD:` a password for the root user *(only for MySQL)*
+ `DB_PORT:` use `3306` for mysql or `5432` for postgres. This value is used for the metabase container.
+ `DB_OUTSIDE_VOLUME:` a name for the database volume. Defaults to `db-data`. If you change this you'll have to modify the `volumes` section in the `docker-compose` file.
+ `DB_INSIDE_VOLUME:` use `/var/lib/mysql` for mysql or `/var/lib/postgresql/data` for postgres. This values are used by the official images inside the container.

Adminer environment:

+ `ADMINER_PORT:` the port where Adminer will be available

Metabase environment:

+ `MB_DB_FILE:` directory where the [metabase file](https://www.metabase.com/docs/latest/operations-guide/running-metabase-on-docker.html#mounting-a-mapped-file-storage-volume) will be written
+ `MB_PORT:` the port where [Metabase](https://www.metabase.com/docs/) will be available
+ `MB_VOLUME:` a name for the metabase volume. Defaults to `mb-data`. If you change this you'll have to modify the `volumes` section in the `docker-compose` file.
+ `MB_JAVA_TIMEZONE:` timezone to match the timezone reports. [More info](https://www.metabase.com/docs/latest/operations-guide/running-metabase-on-docker.html#setting-the-java-timezone) about the Metabase environment. [List](https://garygregory.wordpress.com/2013/06/18/what-are-the-java-timezone-ids/) of available timezone IDs

### Get started

+ Run `docker-compose build` to create a database image (`mysql` or `postgres`)
+ Run `docker-compose up` to run the applications
+ Use `docker-compose down` to shutdown all services
+ Use `docker-compose up` to re-run all services whenever you want
+ There is no need to re-run `docker-compose build` unless you change the database strategy, database name, database credentials or the volumes.

### Change database

Whenever you run a configuration, a docker volume is created for that persistence strategy (`postgres`, `mysql`). The init command in either the postgres or the mysql image try to create a database in the given volume, if the volume exists the initialisation command will fail.

If you later decide to use another database strategy (`postgres`, `mysql`) you will have to either change the `DB_OUTSIDE_VOLUME` value or delete the current volume. Renaming the value also implies modifying the `docker-compose` file in the `volumes` section. You can achieve a volume removal by searching your volume with `docker volume ls`, then remove it `docker volume rm <volume-name>`. Or use simply run this:

+ `docker volume rm $(docker volume ls | grep db-data | awk '{print $2}')`

> `db-data` is the default volume name assigned in `.env.example`. Replace that with your volume name, if you used another value in `.env`.
>

If you can't remove the volume, then it's probably it's in use. Use `docker-compose down` to shut the application down.

Only then you are be able to repeat the instructions to re-run the applications using: `docker-compose build`, then `docker-compose up`.

## Documentation Reference

+ [Metabase](https://www.metabase.com/docs/latest/operations-guide/configuring-application-database.html)

## Contributing

If you'd like to improve or add features on this project feel free to open an [issue](https://github.com/Cambalab/metabase-compose/issues/new).


## License

[**GNU General Public License version 3**](https://opensource.org/licenses/GPL-3.0)

# <Divider>

<p align="center">
  <strong>👩‍💻 with :green_heart: :purple_heart: :heart: by <a href="https://camba.coop" target="_blank" rel="noopener noreferrer"><img width="20" src="http://camba.coop/assets/signature/no_text_logo.png" /> cambá.coop</a> :earth_americas: Buenos Aires, Argentina
  </strong>
</p>
