# TiDB Installation

## Offline Installation
As an example of installing TiDB 5.2.0 locally, the installation process is as follows:
1. Download TiDB.
```shell
curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh
```
2. Declaring the global environment variable
After the installation, TiUP displays the absolute path of the corresponding Shell profile file. You need to modify ${your_shell_profile} in the following source command according to the path.

This command declares the global environment variable:
```shell
source ${your_shell_profile}
```
3. Starting TiDB
This command starts TiDB with 1 TiDB instance, 1 TiKV instance, 1 PD instance, and 1 TiFlash instance:
```shell
tiup playground v5.2.0
```
The default host of TiDB is 127.0.0.1. The default port of TiDB is 4000. The default username of TiDB is "root", with empty password.