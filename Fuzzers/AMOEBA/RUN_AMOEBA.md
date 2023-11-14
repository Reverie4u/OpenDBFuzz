# How to use AMOEBA?

## Install
Firstly, you need to build an image using a Dockerfile of [AMOEBA](https://bit.ly/3I995jL), and the specific command is as follows:
```shell
docker build -t amoeba . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name amoeba -itd  amoeba
```
Lastly, you need to enter the container to use AMOEBA, and the specific command is as follows:
```shell
docker exec -ti amoeba /bin/bash
```
## Fuzzing of PostgreSQL

### Start PostgreSQL
```shell
cd /workspace
eval "$(direnv hook bash)"
start_pg13.sh
```

### Start Fuzzing
```shell
cd /workspace
eval "$(direnv hook bash)"
timeout 1440 ./test_driver.py --workers 30 --output /home/postgres/exp/1 --queries 20000 --rewriter ./calcite-fuzzing --dbms=postgresql --validate --num_loops=100 --feedback=none --dbconf=db_conf_demo.json --query_timeout 30
```
### Output
The output of fuzzing for `PostgreSQL` is located in `/home/postgres/exp/1`.