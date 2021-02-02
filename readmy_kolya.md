## Выключение всех сервисов если они локально установлены на компьютере
- service apache2 stop (выключение веб сервера)
- service redis stop (выключение бд редис)

## Предварительные требования
- Доступ к локальному компьютеру или серверу разработки с Ubuntu 18.04 от имени пользователя sudo без привилегий root. Если вы используете удаленный сервер, рекомендуется установить активный брандмауэр. Для настройки следует использовать [Руководство по начальной настройке сервера на Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).
- Система Docker, установленная на сервере в соответствии с шагами 1 и 2 руководства [Установка и использование Docker в Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04).
- Docker Compose, установленный на сервере в соответствии с шагом 1 руководства [Установка Docker Compose в Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04).

## Полезные команды
- docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' laradock-doc-system_postgres_1 (проверить какой айпи у базы данных или у любой другой базы с помощью имени контейнера)
- docker-compose up laravel-echo-server (зайти в докер контейнер ехо сервера для просмотра логов)
- docker-compose exec redis bash потом redis-cli потом monitor (зайти в докер контейнер редиса и включить мониторинг логов)
- docker-compose exec --user=laradock workspace bash (зайти в докер контейнер рабочего проекта для выполнения в нем команд, от юзера laradock)

## Пошаговая установка
- cd home/user/workspace (перейти в рабочую директорию своей системы)
- git clone https://kolya770@bitbucket.org/kolya770/document-system.git (клонировать проект в рабочую директорию)
- cd document-system (зайти в скачанный проект)
- nano .env (настроить конфиг для той или иной среды, изначально он настроен на локальный контейнер)
- cd laradock (перейти в laradock, контейнер для сборки докера)
- nano .env (настроить конфиг для самого контейнера)
- поднять рабочую среду докера docker-compose up -d nginx mysql phpmyadmin postgres pgadmin redis laravel-echo-server workspace
  docker-compose build --no-cache {container-name}
  docker-compose up -d nginx mysql phpmyadmin redis workspace

- настроить pgAdmin. выдать права на папку sudo chown -R 5050:5050 ~/.laradock/data/pgadmin/ [информация к настройке](https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html#mapped-files-and-directories) 
- перейти на localhost:5050/ зайти в pgAdmin и насторить соеденение серверу, информация находится в .env от laradock
- настроить .env прооекта
- перейти с консоли в рабочий проект docker-compose exec workspace bash
- выполнить composer install
- выполнить npm install && npm run dev
- настроить прада на дериктории(нужно дописать есть что то в трелло)
- выполнить миграцию php artisan migrate
