# How to use Squirrel?

## Install
Firstly, you need to build an image using a Dockerfile of [Squirrel](https://github.com/s3team/Squirrel), and the specific command is as follows:
```shell
docker build -t squirrel . 
```
Secondly, you need to run the image to get a container, and the specific command is as follows:
```shell
# mynet is the name of the docker network, You can access DBMSs in other containers over that network
docker run  --network mynet --name squirrel -itd  squirrel
```
Lastly, you need to enter the container to use Squirrel, and the specific command is as follows:
```shell
docker exec -ti squirrel /bin/bash
```
## Fuzzing of SQLite

### Start Fuzzing
```shell
cd /home/Squirrel/scripts/utils
python3 run.py sqlite /home/Squirrel/data/fuzz_root/input
```
### Output
The output of fuzzing for `SQLite` is located in `/tmp/fuzz`.