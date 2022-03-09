# Quantipin Development stack

Docker development stack for Quantipin

It contains two rootless containers
- (Alpine) PHP-FPM container for the app
- (Alpine) Nginx container that serves as http-proxy to the app

## How to init development environment

1) Run `git clone git@github.com:ironexdev/quantipin.git`

2) Init
- `bin/init`

3) Add following to etc/hosts
- `127.0.0.1 quantipin.local`
- `127.0.0.1 adminer.quantipin.local`
- Or customize the url in docker/images/http-proxy/default.conf

4) Generate Secrets
Base version does not contain any secrets so this step is not needed.
- `bin/docker/compose/secrets`
- This command might complain "No such file or directory", but files with secrets should be created anyway.

5) Start Docker
- `bin/docker/compose/up`

6) Install dependencies
- `bin/docker/app/composer install`

## Command Reference

`bin/init`
- Add bin/docker symlink to docker directory in order to make bin/docker commands work. 

`bin/submodule-update`
- Pull submodules

`bin/docker/app/composer <command>`
- Run composer in app container

`bin/docker/app/console`
- Run Symfony Console command in app container

`bin/docker/app/run <command>`
- Run command on API server

`bin/docker/app/xdebug`
- Set PHP Xdebug mode (https://xdebug.org/docs/all_settings#mode) in app container

`bin/docker/compose/build <service>`
- Build or rebuild services

`bin/docker/compose/cleanup`
- Stop Docker, remove containers and images (optionally add "-v" flag and remove volumes)

`bin/docker/compose/down <service>`
- Stop one or more running services

`bin/docker/compose/restart <service>`
- Restart one or more services

`bin/docker/compose/run <command>`
- Run custom docker-compose command

`bin/docker/compose/secrets`
- Generate Docker secrets

`bin/docker/compose/up`
- Build images if needed and create and start services

`bin/docker/clear-logs`
- Clear app and http-proxy logs
