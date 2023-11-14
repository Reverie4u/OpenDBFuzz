# How to use Troc?

## Install
Firstly, you need to build an image using a Dockerfile of [Troc](https://github.com/tcse-iscas/Troc), and the specific command is as follows:
```shell
docker build -t troc .
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name troc -itd  troc
```
Lastly, you need to enter the container to use Troc, and the specific command is as follows:
```shell
docker exec -ti troc /bin/bash
```

## Fuzzing of MySQL

### Install MYSQL
Firstly, you need to install MySQL, See the detailed tutorial [INSTALL_MySQL.md](https://github.com/Reverie4u/OpenDBFuzz/blob/main/DBMSs/MySQL/INSTALL_MYSQL.md). In order to meet the requirements of Troc, you need to create a user named 'test' in MySQL, and the specific command is as follows:
```shell
mysql -P 3306 -uroot -proot
create user test identified by 'test';
grant all on *.* to 'test'@'%';
```

### Start Fuzzing
After installing MySQL, you can start fuzzing. The specific command is as follows:
1. Fuzz MySQL
```shell
cd /home/troc/target
java -jar troc*.jar --dbms mysql --host mysql-8.0.25 --username test --password test --db test --table t
```
2. Run with a test case
```shell
cd /home/troc/target
java -jar troc*.jar --dbms mysql --host mysql-8.0.25 --username test --password test --db test --set-case --case-file ../cases/test.txt --table t
```
### Output
The output of fuzzing for `MySQL` is located in `/home/troc/target/troc.log`. The following example represents a transaction bug:
```shell
11/07 13:54:23.263 INFO troc.TrocChecker main: Error: Inconsistent query result
11/07 13:54:23.263 INFO troc.TrocChecker main: query: SELECT c3, c0, c2 FROM t WHERE c1
11/07 13:54:23.263 INFO troc.TrocChecker main: =============================BUG REPORT
 -- Create Table SQL: CREATE TABLE t(c0 VARCHAR(13), c1 DOUBLE, c2 INT, c3 TEXT) 
 -- InitializeStatements:
	INSERT IGNORE INTO t(c0, c1) VALUES ("e", 0.7062329216254325);
	INSERT IGNORE INTO t(c3, c1, c2) VALUES ("-2060350018", 0.7187730480912954, -117697251);
	INSERT INTO t(c3, c0, c1, c2) VALUES ("", "Nseo.V^", 0.46733763472335266, -1321475851);
	INSERT IGNORE INTO t(c3, c1, c2) VALUES ("S!sM WSw", 0.21258028277655816, 675127980);
	INSERT INTO t(c3) VALUES ("-1671368370");
	INSERT INTO t(c3, c1) VALUES ("CI", 0.07208791179711704);
	INSERT INTO t(c3, c0, c1, c2) VALUES ("/OmoTS", "0.55483181630", -1.674023892E9, 1001690163);
 -- Initial Table: 
View{
	1:[e, 0.7062329216254325, null, null]
	2:[null, 0.7187730480912954, -117697251, -2060350018]
	3:[Nseo.V^, 0.46733763472335266, -1321475851, ]
	4:[null, 0.21258028277655816, 675127980, S!sM WSw]
	5:[null, null, null, -1671368370]
	6:[null, 0.07208791179711704, null, CI]
	7:[0.55483181630, -1.674023892E9, 1001690163, /OmoTS]
}

 -- Tx1: Transaction{1, REPEATABLE_READ}, with statements:
	BEGIN;
	SELECT c3, c0, c1, c2 FROM t WHERE 0.6530978428558938;
	UPDATE t SET c3="0.015198219684666392" WHERE c2;
	SELECT c3, c0, c2 FROM t WHERE c1;
	COMMIT;

 -- Tx2: Transaction{2, REPEATABLE_READ}, with statements:
	BEGIN;
	DELETE FROM t WHERE (c2) + ((0.4622735003277979) OR (-1.97066774E9));
	SELECT c3, c0, c1, c2 FROM t WHERE (TO_BASE64((-1812710821))) - (1540404559);
	UPDATE t SET c3="V/", c0="0.54710375437", c1=0.5792857157836029, c2=1181455131 WHERE (CAST((-85505553) AS FLOAT)) / (c2);
	SELECT c3, c0, c1 FROM t WHERE (((c3) - (EXP((c1))))IS NULL) >> (((c1) & (-768133623)) OR ((c2) BETWEEN (0.8243399148339549) AND ("-193055139"))) FOR UPDATE;
	COMMIT;

 -- Input Schedule: 1-2-2-1-2-1-2-1-2-2-1
 -- Submitted Order: [1-0, 2-0, 2-1, 1-1, 2-2, 1-2, 2-3, 1-3, 2-4, 2-5, 1-4]
 -- Execution Result: Result:
Order:[1-0, 2-0, 2-1, 1-1, 2-2, 1-2(B), 2-3, 2-4, 2-5, 1-2, 1-3, 1-4]
Query Results:
	1-0: null
	2-0: null
	2-1: null
	1-1: [null, e, 0.7062329216254325, null, -2060350018, null, 0.7187730480912954, -117697251, , Nseo.V^, 0.46733763472335266, -1321475851, S!sM WSw, null, 0.21258028277655816, 675127980, -1671368370, null, null, null, CI, null, 0.07208791179711704, null, /OmoTS, 0.55483181630, -1.674023892E9, 1001690163]
	2-2: [null, e, 0.7062329216254325, null, -1671368370, null, null, null, CI, null, 0.07208791179711704, null]
	1-2: null
	2-3: null
	2-4: []
	2-5: null
	1-2: null
	1-3: [null, e, null, -2060350018, null, -117697251, , Nseo.V^, -1321475851, S!sM WSw, null, 675127980, CI, null, null, /OmoTS, 0.55483181630, 1001690163]
	1-4: null
FinalState: [e, 0.7062329216254325, null, null, null, null, null, -1671368370, null, 0.07208791179711704, null, CI]
DeadBlock: false

 -- Inferred Result: Result:
Order:[1-0, 2-0, 2-1, 1-1, 2-2, 1-2(B), 2-3, 2-4, 2-5, 1-2, 1-3, 1-4]
Query Results:
	1-0: null
	2-0: null
	2-1: null
	1-1: [null, e, 0.7062329216254325, null, -2060350018, null, 0.7187730480912954, -117697251, , Nseo.V^, 0.46733763472335266, -1321475851, S!sM WSw, null, 0.21258028277655816, 675127980, -1671368370, null, null, null, CI, null, 0.07208791179711704, null, /OmoTS, 0.55483181630, -1.674023892E9, 1001690163]
	2-2: [null, e, 0.7062329216254325, null, -1671368370, null, null, null, CI, null, 0.07208791179711704, null]
	1-2: null
	2-3: null
	2-4: []
	2-5: null
	1-2: null
	1-3: [null, e, null, CI, null, null]
	1-4: null
FinalState: [e, 0.7062329216254325, null, null, null, null, null, -1671368370, null, 0.07208791179711704, null, CI]
DeadBlock: false
```
