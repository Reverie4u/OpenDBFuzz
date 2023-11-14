# How to use Apollo?

## Install
Firstly, you need to build an image using a Dockerfile of [Apollo](https://github.com/sslab-gatech/apollo), and the specific command is as follows:
```shell
docker build -t apollo . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name apollo -itd  apollo
```
Lastly, you need to enter the container to use Apollo, and the specific command is as follows:
```shell
docker exec -ti apollo /bin/bash
```
## Fuzzing of PostgreSQL

### Start Fuzzing
```shell
cd /home/apollo/src/sqlfuzz
./fuzz.py -c configuration/postgres.yaml
```
### Output
The output of fuzzing for `PostgreSQL` is located in `/tmp/out`.