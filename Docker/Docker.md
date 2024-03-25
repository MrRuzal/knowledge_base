#Containerization
[**Docker**](https://www.docker.com/) - это открытая платформа для разработки, доставки и запуска приложений.  
  
Docker позволяет отделить приложения от инфраструктуры, что обеспечивает быструю доставку программного обеспечения. С помощью Docker можно управлять инфраструктурой так же, как и приложениями.  
  
Используя методологии Docker для быстрой доставки, тестирования и развертывания кода, можно значительно сократить время между написанием кода и его запуском в производство.  
  
Вы можете загрузить и установить Docker на различных платформах. Обратитесь к следующему разделу и выберите оптимальный для вас путь установки.  

## Docker Commands
### Docker run 
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
```bash
[ OPTIONS ]

-it — интерактивный режим. Перейти в контейнер и запустить внутри контейнера команду
-d — запустить контейнер в фоне (демоном) и вывести его ID
-p port_localhost:port_docker_image — порты из докера на локалхост
-e «TZ=Europe/Moscow» — указываем нашему контейнеру timezone
-h HOSTNAME — присвоить имя хоста контейнеру
— link <имя контейнера> — связать контейнеры с другим
-v /local/path:/container/path/ — прокидываем в контейнер докера директорию с локальной машины
--name CONTAINERNAME — присвоить имя нашему контейнеру
--restart=[no/on-failure/always/unless-stopped] — варианты перезапуска контейнера при крэше
```
```shell
# Для больше читабельности можно при переносе строки ставить символ \
docker run \ 
—restart=always -it \
```
### Простые действия с контейнерами
##### *Создание контейнера*
```bash
docker create -t -i eon01/infinite --name <CONTAINERNAME or CONTAINERID>
```
##### *Переименование контейнера*
```bash
docker rename <OLD CONTAINERNAME> <NEW CONTAINERNAME>
```
##### *Удаление контейнера*
```bash
docker rename <OLD CONTAINERNAME> <NEW CONTAINERNAME>
```
##### *Обновление контейнера*
```bash
docker update --cpu-shares 512 -m 300M <CONTAINERNAME or CONTAINERID>
```
### Запуск и обстановка контейнеров
##### *Запуск остановленного контейнера*
```bash
docker start <CONTAINERNAME>
```
##### *Остановка*
```bash
docker stop <CONTAINERNAME>
```
##### *Перезагрузка*
```bash
docker restart <CONTAINERNAME>
```
##### *Пауза (приостановка всех процессов контейнера)*
```bash
docker pause <CONTAINERNAME>
```
##### *Снятие паузы*
```bash
docker unpause <CONTAINERNAME>
```
##### *Блокировка (до остановки контейнера)*
```bash
docker wait <CONTAINERNAME>
```
##### *Отправка SIGKILL (завершающего сигнала)*
```bash
docker kill <CONTAINERNAME>
```
##### *Отправка другого сигнала*
```bash
docker kill -s HUP <CONTAINERNAME>
```
##### *Подключение к существующему контейнеру*
```bash
docker attach <CONTAINERNAME>
```
Выход через Ctrl+p+q

### Информация о контейнере
#### *Работающие контейнеры*

Вывести работающие контейнеры
```bash
docker ps
```
Вывести все контейнеры
```bash
docker ps -a
```
#### *Логи контейнера*
```bash
docker logs
```
```bash
docker logs <CONTAINERNAME or CONTAINERID>
```
#### *Информация о контейнере*
```bash
# Все параметры контейнера
docker inspect <CONTAINERNAME or CONTAINERID>

# Вывести величину конкретного параметра
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
```
#### *События контейнера*
```bash
docker events <CONTAINERNAME or CONTAINERID>
```
#### *Публичные порты*
```bash
docker port <CONTAINERNAME or CONTAINERID>
```
#### *Выполняющиеся процессы*
```bash
docker top <CONTAINERNAME or CONTAINERID>
```
#### *Использование ресурсов*
```bash
docker stats <CONTAINERNAME or CONTAINERID>
```
#### *Изменения в файлах или директориях файловой системы контейнера*
```bash
docker diff <CONTAINERNAME or CONTAINERID>
```
### Работа c контейнерами
```
`docker compose -f docker-compose.yml up`
```
#### *Зайти в уже запущенный контейнер.*
(точнее выполнить команду внутри контейнера)
```bash
docker exec -it name_of_container /bin/bash
```
#### *Запустить контейнер и открыть в нём bash*
```bash
docker run -it -d --name my_container CONTAINER_ID /bin/bash
```
#### *Копирование файлов внутрь контейнера.*
```bash
docker cp some_files.conf docker_container:/home/docker/
```
При смене путей, можно копировать из контейнера.
### Работа с registry
#### *Вход в реестр*
```shell
# Вход на докерзаб
docker login

# Вход в WebUI локального registry
docker login localhost:8080
```
#### *Выход из реестра*
```shell
# Докерхаб
docker logout

# Локальный registry
docker logout localhost:8080
```
#### *Поиск образа*
```shell
# Простой поиск
docker search nginx

# Поиск с фильтрами
docker search nginx -- filter stars=3 --no-trunc busybox
```
#### *Pull (выгрузка из реестра) образа*
```shell
# Выгрузка из Докерхаба
docker pull nginx

# Из локального registry
docker pull eon01/nginx localhost:5000/myadmin/nginx
```
#### *Push (загрузка в реестр) образа*
```shell
# В докерхаб
docker push eon01/nginx

# В локальный registry
docker push eon01/nginx localhost:5000/myadmin/nginx
```
### Работа с образами
##### *Список образов*
```shell
docker images
```
##### *Создание образов*
(файл опциями сборки образа), учитывая что мы находимся в папке где лежит этот файл. Через ключ -t назначаем имя нашему образу. Точка в конце означает что Dockerfile лежит в текущей директории.
```shell
docker build -t my_docker .
```
```shell
docker build .
```
```shell
docker build github.com/creack/docker-firefox
```
```shell
docker build - < Dockerfile
```
```shell
docker build - < context.tar.gz
```
```shell
docker build -t eon/infinite .
```
```shell
docker build -f myOtherDockerfile .
```
```shell
curl example.com/remote/Dockerfile | docker build -f - .
```
##### *Удаление образа*
```shell
docker rmi imagename (example: nginx)
```
##### *Загрузка репозитория в tar (из файла или стандартного ввода)*
```shell
docker load < ubuntu.tar.gz
```
```shell
docker load --input ubuntu.tar
```
##### *Сохранение образа в tar-архив*
```shell
docker save busybox > ubuntu.tar
```
##### *Просмотр истории образа*
```shell
docker history
```
##### *Создание образа из контейнера*
```shell
docker commit nginx
```
##### *Тегирование образа*
```shell
docker tag nginx eon01/nginx
```
##### *Push (загрузка в реестр) образа*
```shell
docker push eon01/nginx
```
### Работа с томами
- Если монтируем пустой том с хоста, а в контейнере уже есть файлы, то они скопируются в том
- Если монтируем с хоста том с файлами, то они окажутся в контейнере
- Если мы монтируем не пустой том с хоста, а в контейнере по этому пути уже есть файлы, то они будут скрыты
- Можно монтировать с хоста любые файлы. В том числе и служебные. Например сокет docker. Что бы получился docker-in-docker (dind)
##### *Создание тома*
```shell
docker volume create <VOLUME-NAME>
```
##### *Монтируем с хоста в контейнер*
```shell
docker run --mount source=<VOLUME-NAME>,target=/path/to/folder/in/container -d <IMAGE>
```
##### *Монтируем с контейнера на хост*
```shell
docker run --mount type=bind,source=/host/folder,target=/container/folder -d <IMAGE>
```
##### *Посмотреть настройки тома*
```shell
docker volumes inspect <VOLUME-NAME>
```
##### *Вывести список всех томов с их названиями.*
```shell
docker volume ls
```
##### *Удаление volumes по названию*
```shell
docker volume rm <VOLUME-NAME>
```
### Сети
##### *Создание сети*
```shell
docker network create -d overlay MyOverlayNetwork
```
```shell
docker network create -d bridge MyBridgeNetwork
```
```shell
docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork
```
##### *Удаление сети*
```shell
docker network rm MyOverlayNetwork
```
##### *Список сетей*
```shell
docker network ls
```
##### *Получение информации о сети*
```shell
docker network inspect MyOverlayNetwork
```
##### *Подключение работающего контейнера к сети*
```shell
docker network connect MyOverlayNetwork nginx
```
##### *Подключение контейнера к сети при его запуске*
```shell
docker run -it -d --network=MyOverlayNetwork nginx
```
##### *Отключение контейнера от сети*
```shell
docker network disconnect MyOverlayNetwork nginx
```
### Чистка мусора
##### *Показать образы и контейнеры*
```shell
# Вывести все образы (images)
docker images -a

# Вывести все неиспользуемые образы
docker images -f dangling=true

# Выведет список запущенных контейнеров, если добавить ключ -a, выведет список всех контейнеров.
docker ps

```
##### *Удаление образов (images)*
```shell
# Для удаления используется команда docker rmi с добавлением ИД или тега, например:
# с ключом --force удалит контейнер и образ
docker rmi abb461727af5

# Удаление всех образов
docker rmi $(docker images -a -q)

# Удаление всех неиспользуемых образов
docker images prune
docker rmi $(docker images -f dangling=true -q)

# Удаление всех неиспользуемых (не связанных с контейнерами) образов:
# Если добавить к команде ключ -a, то произойдет удаление всех остановленных контейнеров и неиспользуемых образов.
docker system prune

# Удаление всех образов без тегов
docker rmi -f $(docker images | grep "^<none>" | awk "{print $3}")

# Удаление всех образов
docker rmi $(docker images -a -q)
```
##### *Удаление контейнеров (containers)*
```shell
# Для удаления контейнера, его необходимо сначала остановить командой ниже с указанием ID или названия контейнера.
docker stop CONTAINER_ID

# Для удаления контейнера используется команда docker rm с добавлением ИД или названия
docker rm CONTAINER_ID

# Удаление контейнера и его тома (volume)
docker rm -v CONTAINER_ID

# Удаление всех контейнеров со статусом exited
docker rm $(docker ps -a -f status=exited -q)

# Удаление всех остановленных контейнеров
docker container prune
docker rm `docker ps -a -q`

# Удаление контейнеров, остановленных более суток назад
docker container prune --filter "until=24h"

# Остановка и удаление всех контейнеров
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
```
##### *Удаление томов (volumes)*
```shell
#Вывести список всех томов с их названиями.
docker volume ls

# Удаление volumes по названию
docker rm <volume_name>

# Вывести список всех томов не связанных с контейнерами
docker volume ls -f dangling=true

# Удаление томов (volumes) несвязанных с контейнерами
docker volume prune
docker volume rm $(docker volume ls -f dangling=true -q)

# Удаление неиспользуемых (dangling) томов по фильтру
docker volume prune --filter "label!=keep"
```
##### *Удаление сетей (networks)*
```shell
# Вывести список всех сетей с их ИД и названиями. 
docker network ls

# Для удаления используется команда  с добавлением ИД или названия:
docker network rm NETWORK_ID

# Удалит все сети не используемые хотя бы одним контейнером.
docker network prune
```
##### *Удаление всех неиспользуемых объектов*
```shell
docker system prune

# По умолчанию для Docker 17.06.1+ тома не удаляются. Чтобы удалились и они тоже:
docker system prune --volumes
```
## --------------------------------------------------------------

##### *Новый билд:*
```bash
docker-compose up --build
```
##### *Остановки и удаления всех контейнеров, сетей и заного билд:*
```bash
docker-compose down &&  docker-compose up --build
```

```
sudo docker compose -f docker-compose.production.yml down
```
##### *Перезапустить все контейнеры:*
```bash
docker-compose restart
```
##### *Посмотреть список всех активных контейнеров Docker:*
```bash
docker ps
```
Чтобы увидеть список всех контейнеров, включая остановленные, используйте флаг `-a` (или `--all`):
```bash
docker ps -a
```

##### *Удаления неиспользуемых образов:*
```bash
docker image prune --all --force
```

 `docker image prune`: Эта команда начинает процесс удаления неиспользуемых образов.
 `--all`: Этот флаг указывает Docker удалить все неиспользуемые образы, включая те, которые связаны с контейнерами, которые не активны в данный момент.
 `--force`: Этот флаг указывает Docker на принудительное выполнение удаления образов, без запроса подтверждения. Если его не использовать, Docker запросит подтверждение перед удалением.
Таким образом, команда `docker image prune --all --force` удалит все неиспользуемые Docker-образы без дополнительных запросов на подтверждение. (Образы нельзя будет восстановить)

```bash
docker volume prune --all --force
```

```bash
docker builder prune --all --force
```


Остановки и удаления всех контейнеров и сетей:
```bash
docker compose down 
```
*Она также удаляет все связанные с контейнерами ресурсы, такие как объемы данных.*

docker compose

