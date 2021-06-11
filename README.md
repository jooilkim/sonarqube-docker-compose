# sonarqube-docker-compose

Linux

If you're running on Linux, you must ensure that:

* vm.max_map_count is greater than or equal to 524288
* fs.file-max is greater than or equal to 131072
* the user running SonarQube can open at least 131072 file descriptors
* the user running SonarQube can open at least 8192 threads

You can see the values with the following commands:
```
sysctl vm.max_map_count
sysctl fs.file-max
ulimit -n
ulimit -u
```
You can set them dynamically for the current session by running the following commands as root:
```
sysctl -w vm.max_map_count=524288
sysctl -w fs.file-max=131072
ulimit -n 131072
ulimit -u 8192
```
To set these values more permanently, you must update either /etc/sysctl.d/99-sonarqube.conf (or /etc/sysctl.conf as you wish) to reflect these values.

If the user running SonarQube (sonarqube in this example) does not have the permission to have at least 131072 open descriptors, you must insert this line in /etc/security/limits.d/99-sonarqube.conf (or /etc/security/limits.conf as you wish):
```
sonarqube   -   nofile   131072
sonarqube   -   nproc    8192
```
If you are using systemd to start SonarQube, you must specify those limits inside your unit file in the section [service] :
```
[Service]
...
LimitNOFILE=131072
LimitNPROC=8192
...
```
