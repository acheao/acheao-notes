#### redis安装 ####
1. yum install wget
2. mkdir soft
3. cd soft
4. wget https://download.redis.io/releases/redis-6.2.5.tar.gz
5. tar -zxvf redis-6.2.5.tar.gz
6. cd redis-6.2.5
7. make
    1. yum install gcc
    2. make distclean
8. make
9. 此时src已经生成可执行程序
10. make install PREFIX=/opt/redis-6.2.5
11. vi /etc/profile
    - export REDIS_HOME=/opt/redis-6.2.5
    - export PATH=\$PATH:\$REDIS_HOME/bin
    - source /etc/profile
12. cd utils
13. ./install server.sh
14. 如果出现
    ```
    This systems seems to use systemd.
    Please take a look at the provided example service unit files in this directory, and adapt and install them. Sorry!
    ```
    则注释install server.sh
    ```
    #if [ "${_pid_1_exe##*/}" = systemd ]
    #then
    #       echo "This systems seems to use systemd."
    #       echo "Please take a look at the provided example service unit files in this directory, and adapt and install them. Sorry!"
    #       exit 1
    ```
    执行后输出
    ```
    /etc/redis/6381.conf
    Log file       : /var/log/redis_6381.log
    Data dir       : /var/lib/redis/6381
    Executable     : /opt/redis-6.2.5/bin/redis-server
    Cli Executable : /opt/redis-6.2.5/bin/redis-cli
    Is this ok? Then press ENTER to go on or Ctrl-C to abort.
    Copied /tmp/6381.conf => /etc/init.d/redis_6381
    ````
    - 一个物理机中可以有多个redis实例（进程），通过port区分
    - 可执行程序就一份目录，但内存中未来得多个实例需要各自的配置文件，持久化目录
    - service redis_6379 start/stop/status(/etc/init.d/redis_6379)



#### redis命令 ####
1. 查看版本```redis-server --version``` ```redis-server -v```
   
