# OpenDBFuzz
An open-source toolkit for DBMS fuzzing


## Directory Structure
In the root directory, there are two folders:
- `DBMS/`: contains the install ways of DBMSs
- `Fuzzers/`: contains the install ways of Fuzzers, There are a lot of folders named after fuzzer name under this directory, each of which contains a `Dockerfile` for quickly building the runtime environment and usage instructions. 

For examle:
- `Fuzzers/SQLancer/Dockerfile`: Dockerfile to build an image of `SQLancer`
- `Fuzzers/SQLancer/RUN_SQLancer.md`: Usage instructions of `SQLancer`