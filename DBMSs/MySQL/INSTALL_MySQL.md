# MySQL Installation
For black-box fuzzing, there are two methods to install the MySQL database for testing. One is to use Docker images for installation, and the other is to install using an offline installation package.

## Docker Install
Taking the installation of MySQL 8.0.16 as an example, the installation process is as follows:
1. Pull the image from the Docker repository.
```shell
docker pull mysql:8.0.16
```
2. create docker network
```shell
docker network create mynet
```
3. Start docker container
```shell
docker run -p 127.0.0.1:3306:3306 --network mynet --name mysql-8.0.16 -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0.16
````
4. Enter the container
```shell
docker exec -it mysql-8.0.16 bash
```
5. Login to MySQL
```shell
mysql -P 3306 -uroot -proot
```
From this point, a container with MySQL 8.0.16 has been successfully started. When launching a fuzzer container with docker run, include the parameter `--network mynet` to access MySQL 8.0.16 within another container.
## Offline Installation (Deb-Bundle)
As an example of installing MySQL 8.0.16 on Ubuntu 18.04, the installation process is as follows:
1. Install dependencies.
```shell
sudo apt-get -y install wget libaio1 libmecab2 libnuma1 psmisc dialog
```
2. Download the installation package and unzip it.
```shell
mkdir ../mysql && cd ../mysql
wget https://cdn.mysql.com/archives/mysql-8.0/mysql-server_8.0.16-2ubuntu18.04_amd64.deb-bundle.tar
tar xvf mysql-server_8.0.16-2ubuntu18.04_amd64.deb-bundle.tar
```
3. Install MySQL.
```shell
dpkg -i mysql-common_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i libmysqlclient21_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i libmysqlclient-dev_8.0.16-2ubuntu18.04_amd64.deb

dpkg -i mysql-community-client-core_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i mysql-community-client_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i mysql-client_8.0.16-2ubuntu18.04_amd64.deb

dpkg -i mysql-community-server-core_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i mysql-community-server_8.0.16-2ubuntu18.04_amd64.deb
dpkg -i mysql-server_8.0.16-2ubuntu18.04_amd64.deb

mysql -V
```
4. Start MySQL.
```shell
nohup mysqld --user=root &
```

5. Login to MySQL
```shell
mysql -P 3306 -uroot -proot
```