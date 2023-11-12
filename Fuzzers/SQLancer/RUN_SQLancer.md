# How to use SQLancer?

## INSTALL
First, you need to build an image using a Dockerfile, and the specific command is as follows:
```shell
docker build -t sqlancer-18.04 . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
docker run  --network mynet --name sqlancer-18.04 -itd  sqlancer-18.04
```
last, you need to enter the container to use SQLancer, and the specific command is as follows:
```shell
docker exec -ti sqlancer-18.04 /bin/bash
```
## RUN

### Recompile
After entering the container, taking `SQLite` as an example, the first step is to modify the `pom.xml` file located in `/home/sqlancer` to select the SQLite version you want to test. Then, execute the following command to recompile `SQLancer`:
```shell
mvn package -DskipTests
```

### Start Fuzzing
After recompiling `SQLancer`, you can start fuzzing. Taking `SQLite` as an example, the specific command is as follows:
```shell
cd /home/sqlancer/sqlancer/target
java -jar sqlancer-*.jar --timeout-seconds 1440 --num-threads 60 --num-tries 100000000 --print-progress-summary true --use-reducer sqlite3 --oracle PQS 
```
For SQLite, optional oracles include PQS, NOREC, TLP, for more usage instructions, please run the following command:
```shell
cd /home/sqlancer/sqlancer/target
java -jar sqlancer-*.jar --help
```

### Output
The output of fuzzing for `SQLite` is located in `/home/sqlancer/target/logs/sqlite3`. Each `database*.log` file represents a detected logical bug.