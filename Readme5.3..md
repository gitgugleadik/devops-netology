## Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"  
Как сдавать задания  
Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.  

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.  

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.  

Любые вопросы по решению задач задавайте в чате учебной группы.  

# Задача 1  
Сценарий выполения задачи:  

создайте свой репозиторий на https://hub.docker.com;  
выберете любой образ, который содержит веб-сервер Nginx;  
создайте свой fork образа;  
реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:  
<html>  
<head>  
Hey, Netology  
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

https://hub.docker.com/repository/docker/gugleadik/nginx_netology

Примерный набор команд
```bash
docker pull nginx  
docker run -d --name nginx-server -p 80:80 nginx
```
# Nginx заработал  
```bash
docker ps -a  
docker exec -it nginx-server bash  
echo '<html><head>Hey, Netology</head><body><h1>DevOps Engineer!</h1></body></html>' > /usr/share/nginx/html/default.html

docker cp nginx-server:/etc/nginx/nginx.conf nginx.conf

 
> Dockerfile

FROM nginx

COPY nginx.conf /etc/nginx/nginx.conf


RUN echo '<html><head>Hey, Netology</head><body><h1>I’m DevOps Engineer!</h1></body></html>' > /usr/share/nginx/html/default.html

```  
Создал репозиторий gugleadik/nginx_netology на сайте   https://hub.docker.com  
Создал image
```bash
docker build -t nginx_netology .  
docker tag nginx_netology gugleadik/nginx_netology  
docker login -u gugleadik  
docker push gugleadik/nginx_netology:latest  
```
# Задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

Высоконагруженное монолитное java веб-приложение;  
- Можно использовать Docker и горизонтальное масштабирование с подсистемой балансировки нагрузки.  
ссылка  
https://docs.microsoft.com/ru-ru/dotnet/architecture/microservices/architect-microservice-container-applications/containerize-monolithic-applications  
Проблема монолитного приложения, в трудности развития приложения.  

Nodejs веб-приложение;  
 - Также можно использовать Docker, это решение очень удобно будет.  

Мобильное приложение c версиями для Android и iOS;
 - ни один из вариантов, т.к. Когда делают приложение для тлф, всегда делают приложение для Андроид или IOS.  
 Вряд ли возможно делать приложение, чтоб оно содержало в себе версии для АНдроид и IOS это увеличит приложение в разы. Думаю, что делают приложения отдельно друг от друга.  

Шина данных на базе Apache Kafka;  
 - Вполне можно использовать docker, противопоказаний на сайте kafka не нашел.
 - ссылка Отказоустойчивый Kafka кластер в Docker  https://dotsandbrackets.com/highly-available-kafka-cluster-docker-ru/  
 
Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;  

Мониторинг-стек на базе Prometheus и Grafana;  
- можно развернуть на Докере, скорость развертывания, возможность масштабирования для различных задач: 

MongoDB, как основное хранилище данных для java-приложения;  
 - docker Для mongodb существует, но для продакшн систем, если данных много, скорее всего лучше виртуальная машина. 

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.  

# Задача 3
Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;
Добавьте еще один файл в папку /data на хостовой машине;
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
```bash
root@bento:/data# docker run -t -d --name centos1 -v /data:/data centos

docker exec -it centos1 bash
[root@fbb5c7d85bc8 data]# echo 'file centos' > centos_docker.txt
[root@fbb5c7d85bc8 data]# ls -la
total 12
drwxr-xr-x 2 root root 4096 Mar 16 17:54 .
drwxr-xr-x 1 root root 4096 Mar 16 17:40 ..
-rw-r--r-- 1 root root   12 Mar 16 17:54 centos_docker.txt
[root@fbb5c7d85bc8 data]# exit
exit

root@bento:/data# echo 'from host mashine file' > host.txt
root@bento:/data# ls -la
total 16
drwxr-xr-x  2 root root 4096 Mar 16 17:55 .
drwxr-xr-x 23 root root 4096 Mar 16 17:39 ..
-rw-r--r--  1 root root   12 Mar 16 17:54 centos_docker.txt
-rw-r--r--  1 root root   23 Mar 16 17:55 host.txt

root@bento:/data# docker run -t -d --name debian1 -v /data:/data debian
ec53e9d9484e44db594ba47614216e0bd8a9ca2d4c654eac1844ca4d9a2b7705
root@bento:/data#
root@bento:/data# docker exec -it debian1 bash
root@ec53e9d9484e:/# cd data/
root@ec53e9d9484e:/data# ls -la
total 16
drwxr-xr-x 2 root root 4096 Mar 16 17:55 .
drwxr-xr-x 1 root root 4096 Mar 16 17:56 ..
-rw-r--r-- 1 root root   12 Mar 16 17:54 centos_docker.txt
-rw-r--r-- 1 root root   23 Mar 16 17:55 host.txt
```

# Задача 4 (*)
Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.

# Как cдавать задание
Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
