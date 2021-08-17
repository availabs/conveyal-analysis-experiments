# Running Conveyal R5

## System Dependencies

## Java Build Environment

### Preliminaries 1

```sh
sudo apt-get install curl zip unzip
```

The following steps will enable you to build the R5 JAR file.

### Install SDKMAN

The following instructions use [SDKMAN](https://github.com/sdkman/sdkman-cli).

> SDKMAN is a tool for managing parallel Versions of multiple Software
> Development Kits on any Unix based system. It provides a convenient command
> line interface for installing, switching, removing and listing Candidates.

```sh
curl -s https://get.sdkman.io | bash
```

### Install Java

From SDKMAN's usage [page](https://sdkman.io/usage)

```sh
sdk install java
```

### Install Gradle

From Gradle's install [page](https://gradle.org/install/#with-a-package-manager)

```sh
sdk install gradle
```

## Get the source code

### Preliminaries 2

```sh
sudo apt-get install git
```

```sh
git clone https://github.com/conveyal/r5.git
```

## Checkout a stable release (302224c2 = v6.4)

```sh
cd r5
git checkout 302224c2
```

## Build the Java JAR file

```sh
gradle clean build shadowJar -x test
```

The above should create the following file: `build/libs/r5-v6.4.all.jar`

## R5 with MongoDB in Docker

### Fix the configuration file

The _analysis.properties.docker_ file is missing a required option.
Add it with the following command:

```sh
echo 'immediate-shutdown=false' >> analysis.properties.docker
```

### Build the Docker Image

```sh
docker build . --build-arg r5version=v6.4 --tag local-conveyal-r5:6.4
```

## Docker Compose

Replace the existing docker-compose.yml with the following:

```yml
# Usage: docker-compose up
version: "3"
services:
  r5:
    container_name: r5
    image: local-conveyal-r5:6.4
    depends_on:
      - mongo
    links:
      - mongo
    ports:
      - "7070:7070"
  mongo:
    container_name: mongo
    image: mongo
    restart: always
    volumes:
      - mongo-volume:/data/db:rw
    ports:
      - "27017:27017"

volumes:
  mongo-volume:
```

## Starting the R5 Backend

```sh
docker-compose up
```

## Stopping the R5 Backend

1. Press _CTRL-C_
2. Wait for everything to shut down.

```sh
docker-compose down
```

## Securing MongoDB

Note: The above instructions run MongoDB **without** authentication.

To run MongoDB in Docker with authentication, the following steps are required:

1. In _analysis.properties.docker_, include the the authorization creds in thedatabase-uri.
   For example:

   `database-uri=mongodb://root:password@mongo:27017/analysis?authSource=admin`

2. Add the following to the mongo service's configuration in the docker-compose.yml

   ```yml
   environment:
     - MONGO_INITDB_ROOT_USERNAME=root
     - MONGO_INITDB_ROOT_PASSWORD=password
   ```

   Note: The first time running the R5 backend in Docker, its initial attempt to
   connect to MongoDB may fail because the user may not yet be created.
   R5 will re-attempt connection succeed.

3. To run the Conveyal analysis-ui, you must update the MongoDB URL in the .env file:

   `MONGODB_URL=mongodb://root:password@127.0.0.1:27017/analysis?authSource=admin`
