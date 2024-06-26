#Эта директива определяет аргумент DEFAULT_REGISTRY, который устанавливается значением docker.io по умолчанию.
#Этот аргумент может быть переопределен при построении Docker-образа, если необходимо использовать другой реестр Docker.
ARG DEFAULT_REGISTRY=docker.io
#Эта директива определяет аргумент DEFAULT_REPOSITORY, который устанавливается значением library по умолчанию.
#Это используется для указания репозитория по умолчанию в Docker-образе, который будет использоваться в дальнейших инструкциях.
ARG DEFAULT_REPOSITORY=library

#Эта директива начинает новый этап сборки Docker-образа и использует базовый образ maven:3.6.3-jdk-11 из реестра docker.io/library/maven.
#Он называется builder и используется для сборки Java-приложения с помощью Maven.
FROM ${DEFAULT_REGISTRY}/${DEFAULT_REPOSITORY}/maven:3.6.3-jdk-11 as builder

#Эта директива устанавливает рабочий каталог внутри контейнера на /source, где будет происходить сборка приложения.
WORKDIR /source
#Эта директива копирует все файлы из текущего каталога (где находится Dockerfile) внутрь контейнера в рабочий каталог /source.
COPY . /source
#Эта директива выполняет команду mvn clean package внутри контейнера, что запускает Maven для сборки Java-приложения.
RUN mvn clean package

#Эта директива начинает новый этап сборки Docker-образа и использует базовый образ bellsoft/liberica-openjre-alpine:11 из указанного реестра.
#Этот образ содержит OpenJRE для выполнения Java-приложений.
FROM ${DEFAULT_REGISTRY}/bellsoft/liberica-openjre-alpine:11

#Эта директива устанавливает рабочий каталог внутри контейнера на /app, где будет располагаться Java-приложение.
WORKDIR /app

#Эта директива копирует файл demo-0.0.1-SNAPSHOT.jar,
#который был собран на предыдущем этапе (builder), из каталога /source/target/ внутрь контейнера в каталог /app.
COPY --from=builder /source/target/demo-0.0.1-SNAPSHOT.jar /app

#Эта директива объявляет, что контейнер будет прослушивать порт 7000,
#но не привязывает его к какому-либо конкретному порту на хосте.
#Это лишь объявление порта, который приложение внутри контейнера будет слушать.
EXPOSE 7000

#Эта директива определяет команду, которая будет выполнена при запуске контейнера.
#В данном случае, приложение Java будет запущено с помощью команды java -jar demo-0.0.1-SNAPSHOT.jar.
CMD java -jar demo-0.0.1-SNAPSHOT.jar
