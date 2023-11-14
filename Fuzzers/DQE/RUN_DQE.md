# How to use DQE?

## Install
Firstly, you need to build an image using a Dockerfile of [DQE](https://github.com/tcse-iscas/dqetool), and the specific command is as follows:
```shell
docker build -t dqe . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name dqe -itd  dqe
```
Lastly, you need to enter the container to use DQE, and the specific command is as follows:
```shell
docker exec -ti dqe /bin/bash
```
## Fuzzing of SQLite

### Recompile
After entering the container, the first step is to modify the `pom.xml` file located in `/home/dqetool` to select the SQLite version you want to test. Then, execute the following command to recompile `DQE`:
```shell
mvn package -DskipTests
```

### Start Fuzzing
After recompiling `DQE`, you can start fuzzing. The specific command is as follows:
```shell
cd /home/dqetool/target
java -jar dqetool-2.0.0.jar --timeout-seconds 1440 --num-threads 60 --num-tries 100000000 --print-progress-summary true --use-reducer sqlite3 --oracle DQE
```
For more usage instructions, please run the following command:
```shell
cd /home/dqetool/target
java -jar dqetool-2.0.0.jar --help
```

### Output
The output of fuzzing for `SQLite` is located in `/home/dqetool/target/logs/sqlite3`. Each `database*.log` file represents a detected logic bug.

## Fuzzing of MySQL

### Install MYSQL
Firstly, you need to install MySQL, See the detailed tutorial [INSTALL_MySQL.md](https://github.com/Reverie4u/OpenDBFuzz/blob/main/DBMSs/MySQL/INSTALL_MYSQL.md). In order to meet the requirements of SQ Lancer, you need to create a database named 'test' in MySQL, and the specific command is as follows:
```shell
mysql -P 3306 -u root
create database test;
create user test identified by 'test';
grant all on *.* to 'test'@'%';
```

### Start Fuzzing
After installing MySQL, you can start fuzzing. The specific command is as follows:
1. Use MySQL in a different container
```shell
cd /home/dqetool/target
java -jar dqetool-2.0.0.jar --host mysql-8.0.16 --username test --password test  --timeout-seconds 1440 --num-threads 60  --num-tries 100000000  --print-progress-summary true --use-reducer  mysql --oracle PQS
```
2. Use MySQL of this container
```shell
cd /home/dqetool/target
java -jar dqetool-2.0.0.jar  --username test --password test  --timeout-seconds 1440 --num-threads 60  --num-tries 100000000  --print-progress-summary true --use-reducer  mysql --oracle PQS
```
### Output
The output of fuzzing for `MySQL` is located in `/home/dqetool/target/logs/mysql`. Each `database*.log` file represents a detected logic bug.