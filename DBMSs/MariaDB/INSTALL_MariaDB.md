# MariaDB Installation
## Docker Install
Taking the installation of MariaDB 10.5.12 as an example, the installation process is as follows:
1. Pull the image from the Docker repository.
```shell
docker pull mariadb:10.5.12
```
2. create docker network
```shell
docker network create mynet
```
3. Start docker container
```shell
docker run -p 127.0.0.1:10004:3306 --network mynet --name mariadb-10.5.12 -e MARIADB_ROOT_PASSWORD=root -d mariadb:10.5.12
````
4. Enter the container
```shell
docker exec -it mariadb-10.5.12 bash
```
From this point, a container with MariaDB 10.5.12 has been successfully started. When launching a fuzzer container with docker run, include the parameter `--network mynet` to access MariaDB 10.5.12 within another container.