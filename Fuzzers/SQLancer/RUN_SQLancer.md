# How to use SQLancer?

## Install
Firstly, you need to build an image using a Dockerfile of [SQLancer](https://github.com/sqlancer/sqlancer), and the specific command is as follows:
```shell
docker build -t sqlancer-18.04 . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name sqlancer-18.04 -itd  sqlancer-18.04
```
Lastly, you need to enter the container to use SQLancer, and the specific command is as follows:
```shell
docker exec -ti sqlancer-18.04 /bin/bash
```
## Fuzzing of SQLite

### Recompile
After entering the container, the first step is to modify the `pom.xml` file located in `/home/sqlancer` to select the SQLite version you want to test. Then, execute the following command to recompile `SQLancer`:
```shell
mvn package -DskipTests
```

### Start Fuzzing
After recompiling `SQLancer`, you can start fuzzing. The specific command is as follows:
```shell
cd /home/sqlancer/target
java -jar sqlancer-*.jar --timeout-seconds 1440 --num-threads 60 --num-tries 100000000 --print-progress-summary true --use-reducer sqlite3 --oracle PQS 
```
For SQLite, optional oracles include PQS, NOREC, TLP, for more usage instructions, please run the following command:
```shell
cd /home/sqlancer/target
java -jar sqlancer-*.jar --help
```

### Output
The output of fuzzing for `SQLite` is located in `/home/sqlancer/target/logs/sqlite3`. Each `database*.log` file represents a detected logic bug.

## Fuzzing of MySQL

### Install MYSQL
Firstly, you need to install MySQL, See the detailed tutorial [INSTALL_MySQL.md](https://github.com/Reverie4u/OpenDBFuzz/blob/main/DBMSs/MySQL/INSTALL_MYSQL.md). In order to meet the requirements of SQLancer, you need to create a user named 'test' in MySQL, and the specific command is as follows:
```shell
mysql -P 3306 -u root
create database test;
create user test identified by 'test';
grant all on *.* to 'test'@'%';
```

### Start Fuzzing
After installing MySQL, you can start fuzzing. The specific command is as follows:
1. Test MySQL in a different container
```shell
cd /home/sqlancer/target
java -jar sqlancer-*.jar --host mysql-8.0.16 --username test --password test  --timeout-seconds 1440 --num-threads 60  --num-tries 100000000  --print-progress-summary true --use-reducer  mysql --oracle PQS
```
2. Test MySQL of this container
```shell
cd /home/sqlancer/target
java -jar sqlancer-*.jar  --username test --password test  --timeout-seconds 1440 --num-threads 60  --num-tries 100000000  --print-progress-summary true --use-reducer  mysql --oracle PQS
```
### Output
The output of fuzzing for `MySQL` is located in `/home/sqlancer/target/logs/mysql`. Each `database*.log` file represents a detected logic bug.

## Fuzzing of PostgreSQL

### Install PostgreSQL
Firstly, you need to install PostgreSQL, See the detailed tutorial [INSTALL_PostgreSQL.md](https://github.com/Reverie4u/OpenDBFuzz/blob/main/DBMSs/PostgreSQL/INSTALL_PGSQL.md). In order to meet the requirements of SQLancer, you need to change passport of user 'postgres' in PostgreSQL, and the specific command is as follows:
```shell
sudo -i -u postgres
psql -p 5432
ALTER USER postgres PASSWORD 'postgres';
```

### Start Fuzzing
After installing PostgreSQL, you can start fuzzing. The specific command is as follows:

1. Test MySQL of this container
```shell
cd /home/sqlancer/target
java -jar sqlancer-*.jar --port 5432 --username postgres --password postgres  --timeout-seconds 1440 --num-threads 60  --num-tries 100000000  --print-progress-summary true --use-reducer  postgres  --oracle PQS
```
### Output
The output of fuzzing for `PostgreSQL` is located in `/home/sqlancer/target/logs/postgres`. Each `database*.log` file represents a detected logic bug.
