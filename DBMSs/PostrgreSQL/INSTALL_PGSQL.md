# PostgreSQL Installation

## Offline Installation
As an example of installing PostgreSQL 12 on Ubuntu 18.04, the installation process is as follows:
1. Install dependencies.
```shell
sudo apt-get -y install wget lsb-release  ca-certificates sudo vim gnupg
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
apt-get update
```
2. Install PostgreSQL.
```shell
apt-get -y install postgresql-12
```
3. Start PostgreSQL.
```shell
sudo pg_ctlcluster 12 main start
```

5. Login to PostgreSQL
```shell
sudo -i -u postgres
psql -p 5432
```