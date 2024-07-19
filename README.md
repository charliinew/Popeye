# Popeye 💪

You will have to containerize and define the deployment of a simple web poll application.

There are five elements constituting the application:

- Poll, a Flask Python web application that gathers votes and push them into a Redis queue.

- A Redis queue, which holds the votes sent by the Poll application, awaiting for them to be consumed by the Worker.

- The Worker, a Java application which consumes the votes being in the Redis queue, and stores them into a PostgreSQL database.

- A PostgreSQL database, which (persistently) stores the votes stored by the Worker.

- Result, a Node.js web application that fetches the votes from the database and displays the. . . well, result. ;)

# DOCKER IMAGES 🐳

You have to create 3 images.
The specifications for each image are as described below.

# Poll

The image must be based on a Python official image.
The dependencies of the application can be installed using the following command:

```
pip3 install -r requirements.txt
```

The application must expose and run on the port 80 and can be started with:# B-DOP-500-PAR-5-1-popeye-camille.sayous

You will have to containerize and define the deployment of a simple web poll application.

There are five elements constituting the application:

- Poll, a Flask Python web application that gathers votes and push them into a Redis queue.

- A Redis queue, which holds the votes sent by the Poll application, awaiting for them to be consumed by the Worker.

- The Worker, a Java application which consumes the votes being in the Redis queue, and stores them into a PostgreSQL database.

- A PostgreSQL database, which (persistently) stores the votes stored by the Worker.

- Result, a Node.js web application that fetches the votes from the database and displays the. . . well, result. ;)

# DOCKER IMAGES

You have to create 3 images.
The specifications for each image are as described below.

# Poll

The image must be based on a Python official image.
The dependencies of the application can be installed using the following command:

```
pip3 install -r requirements.txt
```

The application must expose and run on the port 80 and can be started with:

```
flask run --host=0.0.0.0 --port=80
```

# Result

The image must be based on an official Node.js version 12 Alpine image.
The application must expose and run on the port 80.
The dependencies of the application can be installed using the following command:

```
npm install
```

# Worker

The image will be built using a multi-stage build.

## FIRST STAGE - COMPILATION

The first stage must be based on maven: _maven:3.9.6-eclipse-temurin-21-alpine_ and be named builder.
It must be used to build (natuurlijk) and package the Worker application using the following commands:

- mvn _dependency:resolve_, from within the folder containing pom.xml;
- then, _mvn package_, from within the folder containing the src folder.

It generates a file in the target folder named _worker-jar-with-dependencies.jar_ (relative to your WORKDIR).

## SECOND STAGE - RUN

The second stage must be based on _eclipse-temurin:21-jre-alpine_.
This is the one really running the worker using the command:

```
java -jar worker-jar-with-dependencies.jar
```

# DOCKER COMPOSE

You now have 3 Dockerfiles that create 3 isolated images. It is now time to make them all work together
using Docker Compose !

Create a _docker-compose.yml_ file that will be responsible for running and linking your containers. You must
use version 3 of Docker Compose’s syntax.

Your Docker Compose file should contain:
**5 services :**

• **poll:**
• builds your _poll_ image;
• redirects port 5000 of the host to the port 80 of the container.

• **redis:**
• uses an existing official image of _Redis_;
• opens port 6379.

• **worker:**
• builds your worker image.

• **db:**
• represents the database that will be used by the apps;
• uses an existing official image of PostgreSQL;
• has its database schema created during container first start.

• **result:**
• builds your result image;
• redirects port 5001 of the host to the port 80 of the container.

**3 networks:**
• _poll-tier_ to allow poll to communicate with _redis_.
• _result-tier_ to allow result to communicate with _db_.
• _back-tier_ to allow worker to communicate with _redis_ and _db_.

**1 named volume:**
• _db-data_ which allows the database’s data to be persistent, if the container dies.

```
flask run --host=0.0.0.0 --port=80
```

# Result

The image must be based on an official Node.js version 12 Alpine image.
The application must expose and run on the port 80.
The dependencies of the application can be installed using the following command:

```
npm install
```

# Worker

The image will be built using a multi-stage build.

## FIRST STAGE - COMPILATION

The first stage must be based on maven: _3.5-jdk-8-alpine_ and be named builder.
It must be used to build (natuurlijk) and package the Worker application using the following commands:

- mvn _dependency:resolve_, from within the folder containing pom.xml;
- then, _mvn package_, from within the folder containing the src folder.

It generates a file in the target folder named _worker-jar-with-dependencies.jar_ (relative to your WORKDIR).

## SECOND STAGE - RUN

The second stage must be based on _openjdk:8-jre-alpine_.
This is the one really running the worker using the command:

```
java -jar worker-jar-with-dependencies.jar
```

# DOCKER COMPOSE

You now have 3 Dockerfiles that create 3 isolated images. It is now time to make them all work together
using Docker Compose !

Create a _docker-compose.yml_ file that will be responsible for running and linking your containers. You must
use version 3 of Docker Compose’s syntax.

Your Docker Compose file should contain:
**5 services :**

• **poll:**
• builds your _poll_ image;
• redirects port 5000 of the host to the port 80 of the container.

• **redis:**
• uses an existing official image of _Redis_;
• opens port 6379.

• **worker:**
• builds your worker image.

• **db:**
• represents the database that will be used by the apps;
• uses an existing official image of PostgreSQL;
• has its database schema created during container first start.

• **result:**
• builds your result image;
• redirects port 5001 of the host to the port 80 of the container.

**3 networks:**
• _poll-tier_ to allow poll to communicate with _redis_.
• _result-tier_ to allow result to communicate with _db_.
• _back-tier_ to allow worker to communicate with _redis_ and _db_.

**1 named volume:**
• _db-data_ which allows the database’s data to be persistent, if the container dies.
