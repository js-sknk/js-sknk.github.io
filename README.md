# Local Development Guide

## Local development with DDEV

### Docker Install

```shell
$ brew install docker --cask
```
도커를 설치한 후에 바로, 도커를 실행한다.

### DDEV Install

```shell
$ brew tap drud/ddev && brew install ddev
$ mkcert -install
```
mkcert 를 설치하면 https 접속시 브라우저가 인증서를 인지한다.
가능하면 브라우저가 모두 설치된 상태에서 mkcert를 설치한다.

### Git clone this repository and Install drupal on DDEV

```shell
$ git clone git@github.com:sknkwoxs/drupal9.git
$ cd drupal9
$ ddev config --project-name=drupal9 --docroot=web --project-type=drupal9
$ vim .ddev/config.yaml

...
name: drupal9
type: drupal9
docroot: web
php_version: "8.0"          // PHP버전을 수정한다.
webserver_type: nginx-fpm
...

:wq                         // 저장하고 빠져나온다.

$ ddev start

...
Creating volume "drupal9-mariadb" with default driver
Building db
Building web
Creating ddev-drupal9-db ... done
Creating ddev-drupal9-dba ... done
Creating ddev-drupal9-web ... done

Creating ddev-router ... done

Ensuring write permissions for drupal9
Ensuring write permissions for drupal9
Existing settings.php file includes settings.ddev.php
Successfully started drupal9
Project can be reached at https://drupal9.ddev.site https://127.0.0.1:32773
...

$ ddev composer install
$ ddev import-db -f=db/db.sql.gz
$ ddev drush uli                    // 어드민 비번 초기화 페이지 접속 url
https://drupal9.ddev.site/en/user/reset/1/1610752483/kHHX-l5QxQ04LGMe50kMyrDyry31Ezk2mWY0OQ37E2c/login
```

### DDEV stop

```shell
$ ddev stop
```

위 명령어로 도커에서 실행중인 드루팔 9 콘테이너를 종료할 수 있다.

`주의: 아파치가 실행중이라면 위 명령어들 실행 중 오류가 날 수 있다. 아파치를 종료 후 위 명령어를 실행해야 한다.`

### DB Import
**config 기능을 통해 보다 쉽게 마이그레이션이 가능하다.** [참조 Using Drupal config  for migration](#using-drupal-config-for-migration)

```shell
$ ddev import-db --src=db/db.sql.gz
```

웹 디렉토리 drupal9 에서 위 명령어를 실행해야 한다.

### DB Export

```shell
$ ddev export-db > db/db.sql.gz
```

웹 디렉토리 drupal9 에서 위 명령어를 실행해야 한다.

### Drush or Drupal Console Execute

```shell
$ ddev drush [명령어]
$ ddev exec drupal [명령어]
```

drupal 명령어는 db와 연동되기 때문에, ddev exec 뒤에 실행해야 도커에서 실행되는 db에 연동되어 정상적으로 동작한다.

### Using Drupal config for migration
#### 방법 1. 환경설정 >> 개발 >> 설정 동기화 메뉴에서 config 파일을 동기화 하여 DB를 생성할 수 있다.
#### 방법 2. drush 명령으로, config 파일을 동기화 할 수 있다.
```shell
$ drush config:import
```
> drush alias 사용으로 명령어를 단축할 수 있다. config:import 의 약어는 cim 이다. [참조](https://www.drush.org/commands/10.x/config_import/)
