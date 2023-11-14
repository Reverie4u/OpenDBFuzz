# How to use SQLRight?

## Install
Firstly, you need to build an image using a Dockerfile of [SQLRight](https://github.com/PSU-Security-Universe/sqlright), and the specific command is as follows:
```shell
docker build -t sqlright . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name sqlright -itd  sqlright
```
Lastly, you need to enter the container to use SQLRight, and the specific command is as follows:
```shell
docker exec -ti sqlright /bin/bash
```
## Fuzzing of SQLite

### Start Fuzzing
```shell
cd /home/sqlite/fuzzing/fuzz_root
bash /home/sqlite/scripts/run_sqlright_sqlite_fuzzing_helper.sh --start-core 0 --num-concurrent 60 -O TLP 
```
### Output
The output of fuzzing for `SQLite` is located in `/home/sqlite/fuzzing/fuzz_root/outputs`.