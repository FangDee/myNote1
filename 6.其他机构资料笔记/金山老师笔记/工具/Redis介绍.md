# [1 介绍](https://jshand.gitee.io/#/course/server/redis?id=_1-介绍)

# [2.安装](https://jshand.gitee.io/#/course/server/redis?id=_2安装)

## [2.1 源码安装](https://jshand.gitee.io/#/course/server/redis?id=_21-源码安装)

安装前 需要安装gcc

```bash
yum install gcc-c++ -y
```

> 使用wget下载源码包
>
> 解压源码包
>
> 进入到源码包，执行 make命令

Download, extract and compile Redis with:

```bash
[root@localhost src]# cd /opt
[root@localhost src]# wget https://download.redis.io/releases/redis-5.0.12.tar.gz
[root@localhost src]# tar xzf redis-5.0.12.tar.gz
[root@localhost src]# cd redis-5.0.12
[root@localhost src]# make MALLOC=libc
```

进入到src目录执行redis-server命令，启动redis

```bash
[root@localhost src]# cd /opt/redis-5.0.12/src/
[root@localhost src]# ./redis-server 
12561:C 02 Jun 2021 09:58:24.999 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
12561:C 02 Jun 2021 09:58:25.000 # Redis version=5.0.12, bits=64, commit=00000000, modified=0, pid=12561, just started
12561:C 02 Jun 2021 09:58:25.000 # Warning: no config file specified, using the default config. In order to specify a config file use ./redis-server /path/to/redis.conf
12561:M 02 Jun 2021 09:58:25.004 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.12 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 12561
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

12561:M 02 Jun 2021 09:58:25.011 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
12561:M 02 Jun 2021 09:58:25.011 # Server initialized
12561:M 02 Jun 2021 09:58:25.011 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
12561:M 02 Jun 2021 09:58:25.011 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
12561:M 02 Jun 2021 09:58:25.013 * DB loaded from disk: 0.001 seconds
12561:M 02 Jun 2021 09:58:25.013 * Ready to accept connections
```

使用redis-cli客户端连接服务器进行测试

```bash
[root@localhost src]# cd /opt/redis-5.0.12/src/
#启动redis-cli客户端，连接 redis服务器
[root@localhost src]# ./redis-cli 

#连接成功 使用命令 set 、get 、keys 命令

#keys * 查看所有的key
127.0.0.1:6379> keys *
1) "age"
2) "name"

#通过set命令设置key-value
127.0.0.1:6379> set classroom 2615
OK

#get命令获取 key对应的值
127.0.0.1:6379> get classroom
"2615"

#keys *
127.0.0.1:6379> keys *
1) "age"
2) "classroom"
3) "name"
127.0.0.1:6379> 
```

使用配置文件，让redis-server后台运行

```bash
# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf

# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.

################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf

################################## MODULES #####################################

# Load modules at startup. If the server is not able to load modules
# it will abort. It is possible to use multiple loadmodule directives.
#
# loadmodule /path/to/my_module.so
# loadmodule /path/to/other_module.so

################################## NETWORK #####################################

# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only into
# the IPv4 loopback interface address (this means Redis will be able to
# accept connections only from clients running into the same computer it
# is running).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# bind 127.0.0.1

# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode no

# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379

# TCP listen() backlog.
#
# In high requests-per-second environments you need an high backlog in order
# to avoid slow clients connections issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlog
# in order to get the desired effect.
tcp-backlog 511

# Unix socket.
#
# Specify the path for the Unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700

# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0

# TCP keepalive.
#
# If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
# of communication. This is useful for two reasons:
#
# 1) Detect dead peers.
# 2) Take the connection alive from the point of view of network
#    equipment in the middle.
#
# On Linux, the specified value (in seconds) is the period used to send ACKs.
# Note that to close the connection the double of the time is needed.
# On other kernels the period depends on the kernel configuration.
#
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised no

# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/run/redis_6379.pid

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""

# To enable logging to the system logger, just set 'syslog-enabled' to yes,
# and optionally update the other syslog parameters to suit your needs.
# syslog-enabled no

# Specify the syslog identity.
# syslog-ident redis

# Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
# syslog-facility local0

# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16

# By default Redis shows an ASCII art logo only when started to log to the
# standard output and if the standard output is a TTY. Basically this means
# that normally a logo is displayed only in interactive sessions.
#
# However it is possible to force the pre-4.0 behavior and always show a
# ASCII art logo in startup logs by setting the following option to yes.
always-show-logo yes

################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1
save 300 10
save 60 10000

# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes

# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

# The filename where to dump the DB
dbfilename dump.rdb

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./

################################# REPLICATION #################################

# Master-Replica replication. Use replicaof to make a Redis instance a copy of
# another Redis server. A few things to understand ASAP about Redis replication.
#
#   +------------------+      +---------------+
#   |      Master      | ---> |    Replica    |
#   | (receive writes) |      |  (exact copy) |
#   +------------------+      +---------------+
#
# 1) Redis replication is asynchronous, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of replicas.
# 2) Redis replicas are able to perform a partial resynchronization with the
#    master if the replication link is lost for a relatively small amount of
#    time. You may want to configure the replication backlog size (see the next
#    sections of this file) with a sensible value depending on your needs.
# 3) Replication is automatic and does not need user intervention. After a
#    network partition replicas automatically try to reconnect to masters
#    and resynchronize with them.
#
# replicaof <masterip> <masterport>

# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the replica to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the replica request.
#
# masterauth <master-password>

# When a replica loses its connection with the master, or when the replication
# is still in progress, the replica can act in two different ways:
#
# 1) if replica-serve-stale-data is set to 'yes' (the default) the replica will
#    still reply to client requests, possibly with out of date data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) if replica-serve-stale-data is set to 'no' the replica will reply with
#    an error "SYNC with master in progress" to all the kind of commands
#    but to INFO, replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,
#    SUBSCRIBE, UNSUBSCRIBE, PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB,
#    COMMAND, POST, HOST: and LATENCY.
#
replica-serve-stale-data yes

# You can configure a replica instance to accept writes or not. Writing against
# a replica instance may be useful to store some ephemeral data (because data
# written on a replica will be easily deleted after resync with the master) but
# may also cause problems if clients are writing to it because of a
# misconfiguration.
#
# Since Redis 2.6 by default replicas are read-only.
#
# Note: read only replicas are not designed to be exposed to untrusted clients
# on the internet. It's just a protection layer against misuse of the instance.
# Still a read only replica exports by default all the administrative commands
# such as CONFIG, DEBUG, and so forth. To a limited extent you can improve
# security of read only replicas using 'rename-command' to shadow all the
# administrative / dangerous commands.
replica-read-only yes

# Replication SYNC strategy: disk or socket.
#
# -------------------------------------------------------
# WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
# -------------------------------------------------------
#
# New replicas and reconnecting replicas that are not able to continue the replication
# process just receiving differences, need to do what is called a "full
# synchronization". An RDB file is transmitted from the master to the replicas.
# The transmission can happen in two different ways:
#
# 1) Disk-backed: The Redis master creates a new process that writes the RDB
#                 file on disk. Later the file is transferred by the parent
#                 process to the replicas incrementally.
# 2) Diskless: The Redis master creates a new process that directly writes the
#              RDB file to replica sockets, without touching the disk at all.
#
# With disk-backed replication, while the RDB file is generated, more replicas
# can be queued and served with the RDB file as soon as the current child producing
# the RDB file finishes its work. With diskless replication instead once
# the transfer starts, new replicas arriving will be queued and a new transfer
# will start when the current one terminates.
#
# When diskless replication is used, the master waits a configurable amount of
# time (in seconds) before starting the transfer in the hope that multiple replicas
# will arrive and the transfer can be parallelized.
#
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
repl-diskless-sync no

# When diskless replication is enabled, it is possible to configure the delay
# the server waits in order to spawn the child that transfers the RDB via socket
# to the replicas.
#
# This is important since once the transfer starts, it is not possible to serve
# new replicas arriving, that will be queued for the next RDB transfer, so the server
# waits a delay in order to let more replicas arrive.
#
# The delay is specified in seconds, and by default is 5 seconds. To disable
# it entirely just set it to 0 seconds and the transfer will start ASAP.
repl-diskless-sync-delay 5

# Replicas send PINGs to server in a predefined interval. It's possible to change
# this interval with the repl_ping_replica_period option. The default value is 10
# seconds.
#
# repl-ping-replica-period 10

# The following option sets the replication timeout for:
#
# 1) Bulk transfer I/O during SYNC, from the point of view of replica.
# 2) Master timeout from the point of view of replicas (data, pings).
# 3) Replica timeout from the point of view of masters (REPLCONF ACK pings).
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-replica-period otherwise a timeout will be detected
# every time there is low traffic between the master and the replica.
#
# repl-timeout 60

# Disable TCP_NODELAY on the replica socket after SYNC?
#
# If you select "yes" Redis will use a smaller number of TCP packets and
# less bandwidth to send data to replicas. But this can add a delay for
# the data to appear on the replica side, up to 40 milliseconds with
# Linux kernels using a default configuration.
#
# If you select "no" the delay for data to appear on the replica side will
# be reduced but more bandwidth will be used for replication.
#
# By default we optimize for low latency, but in very high traffic conditions
# or when the master and replicas are many hops away, turning this to "yes" may
# be a good idea.
repl-disable-tcp-nodelay no

# Set the replication backlog size. The backlog is a buffer that accumulates
# replica data when replicas are disconnected for some time, so that when a replica
# wants to reconnect again, often a full resync is not needed, but a partial
# resync is enough, just passing the portion of data the replica missed while
# disconnected.
#
# The bigger the replication backlog, the longer the time the replica can be
# disconnected and later be able to perform a partial resynchronization.
#
# The backlog is only allocated once there is at least a replica connected.
#
# repl-backlog-size 1mb

# After a master has no longer connected replicas for some time, the backlog
# will be freed. The following option configures the amount of seconds that
# need to elapse, starting from the time the last replica disconnected, for
# the backlog buffer to be freed.
#
# Note that replicas never free the backlog for timeout, since they may be
# promoted to masters later, and should be able to correctly "partially
# resynchronize" with the replicas: hence they should always accumulate backlog.
#
# A value of 0 means to never release the backlog.
#
# repl-backlog-ttl 3600

# The replica priority is an integer number published by Redis in the INFO output.
# It is used by Redis Sentinel in order to select a replica to promote into a
# master if the master is no longer working correctly.
#
# A replica with a low priority number is considered better for promotion, so
# for instance if there are three replicas with priority 10, 100, 25 Sentinel will
# pick the one with priority 10, that is the lowest.
#
# However a special priority of 0 marks the replica as not able to perform the
# role of master, so a replica with priority of 0 will never be selected by
# Redis Sentinel for promotion.
#
# By default the priority is 100.
replica-priority 100

# It is possible for a master to stop accepting writes if there are less than
# N replicas connected, having a lag less or equal than M seconds.
#
# The N replicas need to be in "online" state.
#
# The lag in seconds, that must be <= the specified value, is calculated from
# the last ping received from the replica, that is usually sent every second.
#
# This option does not GUARANTEE that N replicas will accept the write, but
# will limit the window of exposure for lost writes in case not enough replicas
# are available, to the specified number of seconds.
#
# For example to require at least 3 replicas with a lag <= 10 seconds use:
#
# min-replicas-to-write 3
# min-replicas-max-lag 10
#
# Setting one or the other to 0 disables the feature.
#
# By default min-replicas-to-write is set to 0 (feature disabled) and
# min-replicas-max-lag is set to 10.

# A Redis master is able to list the address and port of the attached
# replicas in different ways. For example the "INFO replication" section
# offers this information, which is used, among other tools, by
# Redis Sentinel in order to discover replica instances.
# Another place where this info is available is in the output of the
# "ROLE" command of a master.
#
# The listed IP and address normally reported by a replica is obtained
# in the following way:
#
#   IP: The address is auto detected by checking the peer address
#   of the socket used by the replica to connect with the master.
#
#   Port: The port is communicated by the replica during the replication
#   handshake, and is normally the port that the replica is using to
#   listen for connections.
#
# However when port forwarding or Network Address Translation (NAT) is
# used, the replica may be actually reachable via different IP and port
# pairs. The following two options can be used by a replica in order to
# report to its master a specific set of IP and port, so that both INFO
# and ROLE will report those values.
#
# There is no need to use both the options if you need to override just
# the port or the IP address.
#
# replica-announce-ip 5.5.5.5
# replica-announce-port 1234

################################## SECURITY ###################################

# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared
#requirepass 123456789
# Command renaming.
#
# It is possible to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# hard to guess so that it will still be available for internal-use tools
# but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
# rename-command CONFIG ""
#
# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to replicas may cause problems.

################################### CLIENTS ####################################

# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# maxclients 10000

############################## MEMORY MANAGEMENT ################################

# Set a memory usage limit to the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU or LFU cache, or to
# set a hard memory limit for an instance (using the 'noeviction' policy).
#
# WARNING: If you have replicas attached to an instance with maxmemory on,
# the size of the output buffers needed to feed the replicas are subtracted
# from the used memory count, so that network problems / resyncs will
# not trigger a loop where keys are evicted, and in turn the output
# buffer of replicas is full with DELs of keys evicted triggering the deletion
# of more keys, and so forth until the database is completely emptied.
#
# In short... if you have replicas attached it is suggested that you set a lower
# limit for maxmemory so that there is some free RAM on the system for replica
# output buffers (but this is not needed if the policy is 'noeviction').
#
# maxmemory <bytes>

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> Evict using approximated LRU among the keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU among the keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key among the ones with an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, Redis will return an error on write
#       operations, when there are no suitable keys for eviction.
#
#       At the date of writing these commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# The default is:
#
# maxmemory-policy noeviction

# LRU, LFU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can tune it for speed or
# accuracy. For default Redis will check five keys and pick the one that was
# used less recently, you can change the sample size using the following
# configuration directive.
#
# The default of 5 produces good enough results. 10 Approximates very closely
# true LRU but costs more CPU. 3 is faster but not very accurate.
#
# maxmemory-samples 5

# Starting from Redis 5, by default a replica will ignore its maxmemory setting
# (unless it is promoted to master after a failover or manually). It means
# that the eviction of keys will be just handled by the master, sending the
# DEL commands to the replica as keys evict in the master side.
#
# This behavior ensures that masters and replicas stay consistent, and is usually
# what you want, however if your replica is writable, or you want the replica to have
# a different memory setting, and you are sure all the writes performed to the
# replica are idempotent, then you may change this default (but be sure to understand
# what you are doing).
#
# Note that since the replica by default does not evict, it may end using more
# memory than the one set via maxmemory (there are certain buffers that may
# be larger on the replica, or data structures may sometimes take more memory and so
# forth). So make sure you monitor your replicas and make sure they have enough
# memory to never hit a real out-of-memory condition before the master hits
# the configured maxmemory setting.
#
# replica-ignore-maxmemory yes

############################# LAZY FREEING ####################################

# Redis has two primitives to delete keys. One is called DEL and is a blocking
# deletion of the object. It means that the server stops processing new commands
# in order to reclaim all the memory associated with an object in a synchronous
# way. If the key deleted is associated with a small object, the time needed
# in order to execute the DEL command is very small and comparable to most other
# O(1) or O(log_N) commands in Redis. However if the key is associated with an
# aggregated value containing millions of elements, the server can block for
# a long time (even seconds) in order to complete the operation.
#
# For the above reasons Redis also offers non blocking deletion primitives
# such as UNLINK (non blocking DEL) and the ASYNC option of FLUSHALL and
# FLUSHDB commands, in order to reclaim memory in background. Those commands
# are executed in constant time. Another thread will incrementally free the
# object in the background as fast as possible.
#
# DEL, UNLINK and ASYNC option of FLUSHALL and FLUSHDB are user-controlled.
# It's up to the design of the application to understand when it is a good
# idea to use one or the other. However the Redis server sometimes has to
# delete keys or flush the whole database as a side effect of other operations.
# Specifically Redis deletes objects independently of a user call in the
# following scenarios:
#
# 1) On eviction, because of the maxmemory and maxmemory policy configurations,
#    in order to make room for new data, without going over the specified
#    memory limit.
# 2) Because of expire: when a key with an associated time to live (see the
#    EXPIRE command) must be deleted from memory.
# 3) Because of a side effect of a command that stores data on a key that may
#    already exist. For example the RENAME command may delete the old key
#    content when it is replaced with another one. Similarly SUNIONSTORE
#    or SORT with STORE option may delete existing keys. The SET command
#    itself removes any old content of the specified key in order to replace
#    it with the specified string.
# 4) During replication, when a replica performs a full resynchronization with
#    its master, the content of the whole database is removed in order to
#    load the RDB file just transferred.
#
# In all the above cases the default is to delete objects in a blocking way,
# like if DEL was called. However you can configure each case specifically
# in order to instead release memory in a non-blocking way like if UNLINK
# was called, using the following configuration directives:

lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no

############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.

no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes

# When rewriting the AOF file, Redis is able to use an RDB preamble in the
# AOF file for faster rewrites and recoveries. When this option is turned
# on the rewritten AOF file is composed of two different stanzas:
#
#   [RDB file][AOF tail]
#
# When loading Redis recognizes that the AOF file starts with the "REDIS"
# string and loads the prefixed RDB file, and continues loading the AOF
# tail.
aof-use-rdb-preamble yes

################################ LUA SCRIPTING  ###############################

# Max execution time of a Lua script in milliseconds.
#
# If the maximum execution time is reached Redis will log that a script is
# still in execution after the maximum allowed time and will start to
# reply to queries with an error.
#
# When a long running script exceeds the maximum execution time only the
# SCRIPT KILL and SHUTDOWN NOSAVE commands are available. The first can be
# used to stop a script that did not yet called write commands. The second
# is the only way to shut down the server in the case a write command was
# already issued by the script but the user doesn't want to wait for the natural
# termination of the script.
#
# Set it to 0 or a negative value for unlimited execution without warnings.
lua-time-limit 5000

################################ REDIS CLUSTER  ###############################

# Normal Redis instances can't be part of a Redis Cluster; only nodes that are
# started as cluster nodes can. In order to start a Redis instance as a
# cluster node enable the cluster support uncommenting the following:
#
# cluster-enabled yes

# Every cluster node has a cluster configuration file. This file is not
# intended to be edited by hand. It is created and updated by Redis nodes.
# Every Redis Cluster node requires a different cluster configuration file.
# Make sure that instances running in the same system do not have
# overlapping cluster configuration file names.
#
# cluster-config-file nodes-6379.conf

# Cluster node timeout is the amount of milliseconds a node must be unreachable
# for it to be considered in failure state.
# Most other internal time limits are multiple of the node timeout.
#
# cluster-node-timeout 15000

# A replica of a failing master will avoid to start a failover if its data
# looks too old.
#
# There is no simple way for a replica to actually have an exact measure of
# its "data age", so the following two checks are performed:
#
# 1) If there are multiple replicas able to failover, they exchange messages
#    in order to try to give an advantage to the replica with the best
#    replication offset (more data from the master processed).
#    Replicas will try to get their rank by offset, and apply to the start
#    of the failover a delay proportional to their rank.
#
# 2) Every single replica computes the time of the last interaction with
#    its master. This can be the last ping or command received (if the master
#    is still in the "connected" state), or the time that elapsed since the
#    disconnection with the master (if the replication link is currently down).
#    If the last interaction is too old, the replica will not try to failover
#    at all.
#
# The point "2" can be tuned by user. Specifically a replica will not perform
# the failover if, since the last interaction with the master, the time
# elapsed is greater than:
#
#   (node-timeout * replica-validity-factor) + repl-ping-replica-period
#
# So for example if node-timeout is 30 seconds, and the replica-validity-factor
# is 10, and assuming a default repl-ping-replica-period of 10 seconds, the
# replica will not try to failover if it was not able to talk with the master
# for longer than 310 seconds.
#
# A large replica-validity-factor may allow replicas with too old data to failover
# a master, while a too small value may prevent the cluster from being able to
# elect a replica at all.
#
# For maximum availability, it is possible to set the replica-validity-factor
# to a value of 0, which means, that replicas will always try to failover the
# master regardless of the last time they interacted with the master.
# (However they'll always try to apply a delay proportional to their
# offset rank).
#
# Zero is the only value able to guarantee that when all the partitions heal
# the cluster will always be able to continue.
#
# cluster-replica-validity-factor 10

# Cluster replicas are able to migrate to orphaned masters, that are masters
# that are left without working replicas. This improves the cluster ability
# to resist to failures as otherwise an orphaned master can't be failed over
# in case of failure if it has no working replicas.
#
# Replicas migrate to orphaned masters only if there are still at least a
# given number of other working replicas for their old master. This number
# is the "migration barrier". A migration barrier of 1 means that a replica
# will migrate only if there is at least 1 other working replica for its master
# and so forth. It usually reflects the number of replicas you want for every
# master in your cluster.
#
# Default is 1 (replicas migrate only if their masters remain with at least
# one replica). To disable migration just set it to a very large value.
# A value of 0 can be set but is useful only for debugging and dangerous
# in production.
#
# cluster-migration-barrier 1

# By default Redis Cluster nodes stop accepting queries if they detect there
# is at least an hash slot uncovered (no available node is serving it).
# This way if the cluster is partially down (for example a range of hash slots
# are no longer covered) all the cluster becomes, eventually, unavailable.
# It automatically returns available as soon as all the slots are covered again.
#
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes

# This option, when set to yes, prevents replicas from trying to failover its
# master during master failures. However the master can still perform a
# manual failover, if forced to do so.
#
# This is useful in different scenarios, especially in the case of multiple
# data center operations, where we want one side to never be promoted if not
# in the case of a total DC failure.
#
# cluster-replica-no-failover no

# In order to setup your cluster make sure to read the documentation
# available at http://redis.io web site.

########################## CLUSTER DOCKER/NAT support  ########################

# In certain deployments, Redis Cluster nodes address discovery fails, because
# addresses are NAT-ted or because ports are forwarded (the typical case is
# Docker and other containers).
#
# In order to make Redis Cluster working in such environments, a static
# configuration where each node knows its public address is needed. The
# following two options are used for this scope, and are:
#
# * cluster-announce-ip
# * cluster-announce-port
# * cluster-announce-bus-port
#
# Each instruct the node about its address, client port, and cluster message
# bus port. The information is then published in the header of the bus packets
# so that other nodes will be able to correctly map the address of the node
# publishing the information.
#
# If the above options are not used, the normal Redis Cluster auto-detection
# will be used instead.
#
# Note that when remapped, the bus port may not be at the fixed offset of
# clients port + 10000, so you can specify any port and bus-port depending
# on how they get remapped. If the bus-port is not set, a fixed offset of
# 10000 will be used as usually.
#
# Example:
#
# cluster-announce-ip 10.1.1.5
# cluster-announce-port 6379
# cluster-announce-bus-port 6380

################################## SLOW LOG ###################################

# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.

# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000

# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 128

################################ LATENCY MONITOR ##############################

# The Redis latency monitoring subsystem samples different operations
# at runtime in order to collect data related to possible sources of
# latency of a Redis instance.
#
# Via the LATENCY command this information is available to the user that can
# print graphs and obtain reports.
#
# The system only logs operations that were performed in a time equal or
# greater than the amount of milliseconds specified via the
# latency-monitor-threshold configuration directive. When its value is set
# to zero, the latency monitor is turned off.
#
# By default latency monitoring is disabled since it is mostly not needed
# if you don't have latency issues, and collecting data has a performance
# impact, that while very small, can be measured under big load. Latency
# monitoring can easily be enabled at runtime using the command
# "CONFIG SET latency-monitor-threshold <milliseconds>" if needed.
latency-monitor-threshold 0

############################# EVENT NOTIFICATION ##############################

# Redis can notify Pub/Sub clients about events happening in the key space.
# This feature is documented at http://redis.io/topics/notifications
#
# For instance if keyspace events notification is enabled, and a client
# performs a DEL operation on key "foo" stored in the Database 0, two
# messages will be published via Pub/Sub:
#
# PUBLISH __keyspace@0__:foo del
# PUBLISH __keyevent@0__:del foo
#
# It is possible to select the events that Redis will notify among a set
# of classes. Every class is identified by a single character:
#
#  K     Keyspace events, published with __keyspace@<db>__ prefix.
#  E     Keyevent events, published with __keyevent@<db>__ prefix.
#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
#  $     String commands
#  l     List commands
#  s     Set commands
#  h     Hash commands
#  z     Sorted set commands
#  x     Expired events (events generated every time a key expires)
#  e     Evicted events (events generated when a key is evicted for maxmemory)
#  A     Alias for g$lshzxe, so that the "AKE" string means all the events.
#
#  The "notify-keyspace-events" takes as argument a string that is composed
#  of zero or multiple characters. The empty string means that notifications
#  are disabled.
#
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
#
#  By default all notifications are disabled because most users don't need
#  this feature and the feature has some overhead. Note that if you don't
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events ""

############################### ADVANCED CONFIG ###############################

# Hashes are encoded using a memory efficient data structure when they have a
# small number of entries, and the biggest entry does not exceed a given
# threshold. These thresholds can be configured using the following directives.
hash-max-ziplist-entries 512
hash-max-ziplist-value 64

# Lists are also encoded in a special way to save a lot of space.
# The number of entries allowed per internal list node can be specified
# as a fixed maximum size or a maximum number of elements.
# For a fixed maximum size, use -5 through -1, meaning:
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
# Positive numbers mean store up to _exactly_ that number of elements
# per list node.
# The highest performing option is usually -2 (8 Kb size) or -1 (4 Kb size),
# but if your use case is unique, adjust the settings as necessary.
list-max-ziplist-size -2

# Lists may also be compressed.
# Compress depth is the number of quicklist ziplist nodes from *each* side of
# the list to *exclude* from compression.  The head and tail of the list
# are always uncompressed for fast push/pop operations.  Settings are:
# 0: disable all list compression
# 1: depth 1 means "don't start compressing until after 1 node into the list,
#    going from either the head or tail"
#    So: [head]->node->node->...->node->[tail]
#    [head], [tail] will always be uncompressed; inner nodes will compress.
# 2: [head]->[next]->node->node->...->node->[prev]->[tail]
#    2 here means: don't compress head or head->next or tail->prev or tail,
#    but compress all nodes between them.
# 3: [head]->[next]->[next]->node->node->...->node->[prev]->[prev]->[tail]
# etc.
list-compress-depth 0

# Sets have a special encoding in just one case: when a set is composed
# of just strings that happen to be integers in radix 10 in the range
# of 64 bit signed integers.
# The following configuration setting sets the limit in the size of the
# set in order to use this special memory saving encoding.
set-max-intset-entries 512

# Similarly to hashes and lists, sorted sets are also specially encoded in
# order to save a lot of space. This encoding is only used when the length and
# elements of a sorted set are below the following limits:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64

# HyperLogLog sparse representation bytes limit. The limit includes the
# 16 bytes header. When an HyperLogLog using the sparse representation crosses
# this limit, it is converted into the dense representation.
#
# A value greater than 16000 is totally useless, since at that point the
# dense representation is more memory efficient.
#
# The suggested value is ~ 3000 in order to have the benefits of
# the space efficient encoding without slowing down too much PFADD,
# which is O(N) with the sparse encoding. The value can be raised to
# ~ 10000 when CPU is not a concern, but space is, and the data set is
# composed of many HyperLogLogs with cardinality in the 0 - 15000 range.
hll-sparse-max-bytes 3000

# Streams macro node max size / items. The stream data structure is a radix
# tree of big nodes that encode multiple items inside. Using this configuration
# it is possible to configure how big a single node can be in bytes, and the
# maximum number of items it may contain before switching to a new node when
# appending new stream entries. If any of the following settings are set to
# zero, the limit is ignored, so for instance it is possible to set just a
# max entires limit by setting max-bytes to 0 and max-entries to the desired
# value.
stream-node-max-bytes 4096
stream-node-max-entries 100

# Active rehashing uses 1 millisecond every 100 milliseconds of CPU time in
# order to help rehashing the main Redis hash table (the one mapping top-level
# keys to values). The hash table implementation Redis uses (see dict.c)
# performs a lazy rehashing: the more operation you run into a hash table
# that is rehashing, the more rehashing "steps" are performed, so if the
# server is idle the rehashing is never complete and some more memory is used
# by the hash table.
#
# The default is to use this millisecond 10 times every second in order to
# actively rehash the main dictionaries, freeing memory when possible.
#
# If unsure:
# use "activerehashing no" if you have hard latency requirements and it is
# not a good thing in your environment that Redis can reply from time to time
# to queries with 2 milliseconds delay.
#
# use "activerehashing yes" if you don't have such hard requirements but
# want to free memory asap when possible.
activerehashing yes

# The client output buffer limits can be used to force disconnection of clients
# that are not reading data from the server fast enough for some reason (a
# common reason is that a Pub/Sub client can't consume messages as fast as the
# publisher can produce them).
#
# The limit can be set differently for the three different classes of clients:
#
# normal -> normal clients including MONITOR clients
# replica  -> replica clients
# pubsub -> clients subscribed to at least one pubsub channel or pattern
#
# The syntax of every client-output-buffer-limit directive is the following:
#
# client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>
#
# A client is immediately disconnected once the hard limit is reached, or if
# the soft limit is reached and remains reached for the specified number of
# seconds (continuously).
# So for instance if the hard limit is 32 megabytes and the soft limit is
# 16 megabytes / 10 seconds, the client will get disconnected immediately
# if the size of the output buffers reach 32 megabytes, but will also get
# disconnected if the client reaches 16 megabytes and continuously overcomes
# the limit for 10 seconds.
#
# By default normal clients are not limited because they don't receive data
# without asking (in a push way), but just after a request, so only
# asynchronous clients may create a scenario where data is requested faster
# than it can read.
#
# Instead there is a default limit for pubsub and replica clients, since
# subscribers and replicas receive data in a push fashion.
#
# Both the hard or the soft limit can be disabled by setting them to zero.
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

# Client query buffers accumulate new commands. They are limited to a fixed
# amount by default in order to avoid that a protocol desynchronization (for
# instance due to a bug in the client) will lead to unbound memory usage in
# the query buffer. However you can configure it here if you have very special
# needs, such us huge multi/exec requests or alike.
#
# client-query-buffer-limit 1gb

# In the Redis protocol, bulk requests, that are, elements representing single
# strings, are normally limited ot 512 mb. However you can change this limit
# here.
#
# proto-max-bulk-len 512mb

# Redis calls an internal function to perform many background tasks, like
# closing connections of clients in timeout, purging expired keys that are
# never requested, and so forth.
#
# Not all tasks are performed with the same frequency, but Redis checks for
# tasks to perform according to the specified "hz" value.
#
# By default "hz" is set to 10. Raising the value will use more CPU when
# Redis is idle, but at the same time will make Redis more responsive when
# there are many keys expiring at the same time, and timeouts may be
# handled with more precision.
#
# The range is between 1 and 500, however a value over 100 is usually not
# a good idea. Most users should use the default of 10 and raise this up to
# 100 only in environments where very low latency is required.
hz 10

# Normally it is useful to have an HZ value which is proportional to the
# number of clients connected. This is useful in order, for instance, to
# avoid too many clients are processed for each background task invocation
# in order to avoid latency spikes.
#
# Since the default HZ value by default is conservatively set to 10, Redis
# offers, and enables by default, the ability to use an adaptive HZ value
# which will temporary raise when there are many connected clients.
#
# When dynamic HZ is enabled, the actual configured HZ will be used as
# as a baseline, but multiples of the configured HZ value will be actually
# used as needed once more clients are connected. In this way an idle
# instance will use very little CPU time while a busy instance will be
# more responsive.
dynamic-hz yes

# When a child rewrites the AOF file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
aof-rewrite-incremental-fsync yes

# When redis saves RDB file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
rdb-save-incremental-fsync yes

# Redis LFU eviction (see maxmemory setting) can be tuned. However it is a good
# idea to start with the default settings and only change them after investigating
# how to improve the performances and how the keys LFU change over time, which
# is possible to inspect via the OBJECT FREQ command.
#
# There are two tunable parameters in the Redis LFU implementation: the
# counter logarithm factor and the counter decay time. It is important to
# understand what the two parameters mean before changing them.
#
# The LFU counter is just 8 bits per key, it's maximum value is 255, so Redis
# uses a probabilistic increment with logarithmic behavior. Given the value
# of the old counter, when a key is accessed, the counter is incremented in
# this way:
#
# 1. A random number R between 0 and 1 is extracted.
# 2. A probability P is calculated as 1/(old_value*lfu_log_factor+1).
# 3. The counter is incremented only if R < P.
#
# The default lfu-log-factor is 10. This is a table of how the frequency
# counter changes with a different number of accesses with different
# logarithmic factors:
#
# +--------+------------+------------+------------+------------+------------+
# | factor | 100 hits   | 1000 hits  | 100K hits  | 1M hits    | 10M hits   |
# +--------+------------+------------+------------+------------+------------+
# | 0      | 104        | 255        | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 1      | 18         | 49         | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 10     | 10         | 18         | 142        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 100    | 8          | 11         | 49         | 143        | 255        |
# +--------+------------+------------+------------+------------+------------+
#
# NOTE: The above table was obtained by running the following commands:
#
#   redis-benchmark -n 1000000 incr foo
#   redis-cli object freq foo
#
# NOTE 2: The counter initial value is 5 in order to give new objects a chance
# to accumulate hits.
#
# The counter decay time is the time, in minutes, that must elapse in order
# for the key counter to be divided by two (or decremented if it has a value
# less <= 10).
#
# The default value for the lfu-decay-time is 1. A Special value of 0 means to
# decay the counter every time it happens to be scanned.
#
# lfu-log-factor 10
# lfu-decay-time 1

########################### ACTIVE DEFRAGMENTATION #######################
#
# WARNING THIS FEATURE IS EXPERIMENTAL. However it was stress tested
# even in production and manually tested by multiple engineers for some
# time.
#
# What is active defragmentation?
# -------------------------------
#
# Active (online) defragmentation allows a Redis server to compact the
# spaces left between small allocations and deallocations of data in memory,
# thus allowing to reclaim back memory.
#
# Fragmentation is a natural process that happens with every allocator (but
# less so with Jemalloc, fortunately) and certain workloads. Normally a server
# restart is needed in order to lower the fragmentation, or at least to flush
# away all the data and create it again. However thanks to this feature
# implemented by Oran Agra for Redis 4.0 this process can happen at runtime
# in an "hot" way, while the server is running.
#
# Basically when the fragmentation is over a certain level (see the
# configuration options below) Redis will start to create new copies of the
# values in contiguous memory regions by exploiting certain specific Jemalloc
# features (in order to understand if an allocation is causing fragmentation
# and to allocate it in a better place), and at the same time, will release the
# old copies of the data. This process, repeated incrementally for all the keys
# will cause the fragmentation to drop back to normal values.
#
# Important things to understand:
#
# 1. This feature is disabled by default, and only works if you compiled Redis
#    to use the copy of Jemalloc we ship with the source code of Redis.
#    This is the default with Linux builds.
#
# 2. You never need to enable this feature if you don't have fragmentation
#    issues.
#
# 3. Once you experience fragmentation, you can enable this feature when
#    needed with the command "CONFIG SET activedefrag yes".
#
# The configuration parameters are able to fine tune the behavior of the
# defragmentation process. If you are not sure about what they mean it is
# a good idea to leave the defaults untouched.

# Enabled active defragmentation
# activedefrag yes

# Minimum amount of fragmentation waste to start active defrag
# active-defrag-ignore-bytes 100mb

# Minimum percentage of fragmentation to start active defrag
# active-defrag-threshold-lower 10

# Maximum percentage of fragmentation at which we use maximum effort
# active-defrag-threshold-upper 100

# Minimal effort for defrag in CPU percentage
# active-defrag-cycle-min 5

# Maximal effort for defrag in CPU percentage
# active-defrag-cycle-max 75

# Maximum number of set/hash/zset/list fields that will be processed from
# the main dictionary scan
# active-defrag-max-scan-fields 1000
```

### [配置文件的详解](https://jshand.gitee.io/#/course/server/redis?id=配置文件的详解)

```bash
###################################  NETWORK  ###################################

# 指定 redis 只接收来自于该IP地址的请求，如果不进行设置，那么将处理所有请求
bind 127.0.0.1

#是否开启保护模式，默认开启。要是配置里没有指定bind和密码。开启该参数后，redis只会本地进行访问，
#拒绝外部访问。要是开启了密码和bind，可以开启。否则最好关闭，设置为no
protected-mode yes

#redis监听的端口号
port 6379

#此参数确定了TCP连接中已完成队列(完成三次握手之后)的长度， 当然此值必须不大于Linux系统定义
#的/proc/sys/net/core/somaxconn值，默认是511，而Linux的默认参数值是128。当系统并发量大并且客户端
#速度缓慢的时候，可以将这二个参数一起参考设定。该内核参数默认值一般是128，对于负载很大的服务程序来说
#大大的不够。一般会将它修改为2048或者更大。在/etc/sysctl.conf中添加:net.core.somaxconn = 2048，
#然后在终端中执行sysctl -p
tcp-backlog 511

#此参数为设置客户端空闲超过timeout，服务端会断开连接，为0则服务端不会主动断开连接，不能小于0
timeout 0

#tcp keepalive参数。如果设置不为0，就使用配置tcp的SO_KEEPALIVE值，使用keepalive有两个好处:检测挂
#掉的对端。降低中间设备出问题而导致网络看似连接却已经与对端端口的问题。在Linux内核中，设置了
keepalive，redis会定时给对端发送ack。检测到对端关闭需要两倍的设置值
tcp-keepalive 300

#是否在后台执行，yes：后台运行；no：不是后台运行
daemonize yes

#redis的进程文件
pidfile /var/run/redis/redis.pid

#指定了服务端日志的级别。级别包括：debug（很多信息，方便开发、测试），verbose（许多有用的信息，
#但是没有debug级别信息多），notice（适当的日志级别，适合生产环境），warn（只有非常重要的信息）
loglevel notice

#指定了记录日志的文件。空字符串的话，日志会打印到标准输出设备。后台运行的redis标准输出是/dev/null
logfile /usr/local/redis/var/redis.log


#是否打开记录syslog功能
# syslog-enabled no

#syslog的标识符。
# syslog-ident redis

#日志的来源、设备
# syslog-facility local0

#数据库的数量，默认使用的数据库是0。可以通过”SELECT 【数据库序号】“命令选择一个数据库，序号从0开始
databases 16











#################################  SNAPSHOTTING  ################################# 

#RDB核心规则配置 save <指定时间间隔> <执行指定次数更新操作>，满足条件就将内存中的数据同步到硬盘
#中。官方出厂配置默认是 900秒内有1个更改，300秒内有10个更改以及60秒内有10000个更改，则将内存中的
#数据快照写入磁盘。
#若不想用RDB方案，可以把 save "" 的注释打开，下面三个注释
#   save ""
save 900 1
save 300 10
save 60 10000

#当RDB持久化出现错误后，是否依然进行继续进行工作，yes：不能进行工作，no：可以继续进行工作，可以通
#过info中的rdb_last_bgsave_status了解RDB持久化是否有错误
stop-writes-on-bgsave-error yes

#配置存储至本地数据库时是否压缩数据，默认为yes。Redis采用LZF压缩方式，但占用了一点CPU的时间。若关闭该选项，
#但会导致数据库文件变的巨大。建议开启。
rdbcompression yes

#是否校验rdb文件;从rdb格式的第五个版本开始，在rdb文件的末尾会带上CRC64的校验和。这跟有利于文件的
#容错性，但是在保存rdb文件的时候，会有大概10%的性能损耗，所以如果你追求高性能，可以关闭该配置
rdbchecksum yes

#指定本地数据库文件名，一般采用默认的 dump.rdb
dbfilename dump.rdb

#数据目录，数据库的写入会在这个目录。rdb、aof文件也会写在这个目录
dir /usr/local/redis/var











################################# REPLICATION #################################

# 复制选项，slave复制对应的master。
# replicaof <masterip> <masterport>

#如果master设置了requirepass，那么slave要连上master，需要有master的密码才行。masterauth就是用来
#配置master的密码，这样可以在连上master后进行认证。
# masterauth <master-password>

#当从库同主机失去连接或者复制正在进行，从机库有两种运行方式：1) 如果slave-serve-stale-data设置为
#yes(默认设置)，从库会继续响应客户端的请求。2) 如果slave-serve-stale-data设置为no，
#INFO,replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,SUBSCRIBE, UNSUBSCRIBE,
#PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB,COMMAND, POST, HOST: and LATENCY命令之外的任何请求
#都会返回一个错误”SYNC with master in progress”。
replica-serve-stale-data yes

#作为从服务器，默认情况下是只读的（yes），可以修改成NO，用于写（不建议）
#replica-read-only yes

# 是否使用socket方式复制数据。目前redis复制提供两种方式，disk和socket。如果新的slave连上来或者
#重连的slave无法部分同步，就会执行全量同步，master会生成rdb文件。有2种方式：disk方式是master创建
#一个新的进程把rdb文件保存到磁盘，再把磁盘上的rdb文件传递给slave。socket是master创建一个新的进
#程，直接把rdb文件以socket的方式发给slave。disk方式的时候，当一个rdb保存的过程中，多个slave都能
#共享这个rdb文件。socket的方式就的一个个slave顺序复制。在磁盘速度缓慢，网速快的情况下推荐用socket方式。
repl-diskless-sync no

#diskless复制的延迟时间，防止设置为0。一旦复制开始，节点不会再接收新slave的复制请求直到下一个rdb传输。
#所以最好等待一段时间，等更多的slave连上来
repl-diskless-sync-delay 5

#slave根据指定的时间间隔向服务器发送ping请求。时间间隔可以通过 repl_ping_slave_period 来设置，默认10秒。
# repl-ping-slave-period 10

#复制连接超时时间。master和slave都有超时时间的设置。master检测到slave上次发送的时间超过repl-timeout，即认为slave离线，清除该slave信息。slave检测到上次和master交互的时间#超过repl-timeout，则认为master离线。需要注意的是repl-timeout需要设置一个比repl-ping-slave-period更大的值，不然会经常检测到超时
# repl-timeout 60


#是否禁止复制tcp链接的tcp nodelay参数，可传递yes或者no。默认是no，即使用tcp nodelay。如果
#master设置了yes来禁止tcp nodelay设置，在把数据复制给slave的时候，会减少包的数量和更小的网络带
#宽。但是这也可能带来数据的延迟。默认我们推荐更小的延迟，但是在数据量传输很大的场景下，建议选择yes
repl-disable-tcp-nodelay no

#复制缓冲区大小，这是一个环形复制缓冲区，用来保存最新复制的命令。这样在slave离线的时候，不需要完
#全复制master的数据，如果可以执行部分同步，只需要把缓冲区的部分数据复制给slave，就能恢复正常复制状
#态。缓冲区的大小越大，slave离线的时间可以更长，复制缓冲区只有在有slave连接的时候才分配内存。没有
#slave的一段时间，内存会被释放出来，默认1m
# repl-backlog-size 1mb

# master没有slave一段时间会释放复制缓冲区的内存，repl-backlog-ttl用来设置该时间长度。单位为秒。
# repl-backlog-ttl 3600

# 当master不可用，Sentinel会根据slave的优先级选举一个master。最低的优先级的slave，当选master。
#而配置成0，永远不会被选举
replica-priority 100

#redis提供了可以让master停止写入的方式，如果配置了min-replicas-to-write，健康的slave的个数小于N，mater就禁止写入。master最少得有多少个健康的slave存活才能执行写命令。这个#配置虽然不能保证N个slave都一定能接收到master的写操作，但是能避免没有足够健康的slave的时候，master不能写入来避免数据丢失。设置为0是关闭该功能
# min-replicas-to-write 3

# 延迟小于min-replicas-max-lag秒的slave才认为是健康的slave
# min-replicas-max-lag 10

# 设置1或另一个设置为0禁用这个特性。
# Setting one or the other to 0 disables the feature.
# By default min-replicas-to-write is set to 0 (feature disabled) and
# min-replicas-max-lag is set to 10.







#################################  SECURITY #################################
#网络中。为了保持向后的兼容性，可以注释该命令，因为大部分用户也不需要认证。使用requirepass的时候需要
#注意，因为redis太快了，每秒可以认证15w次密码，简单的密码很容易被攻破，所以最好使用一个更复杂的密码
# requirepass foobared
#把危险的命令给修改成其他名称。比如CONFIG命令可以重命名为一个很难被猜到的命令，这样用户不能使用，而
#内部工具还能接着使用
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#设置成一个空的值，可以禁止一个命令
# rename-command CONFIG ""








 #################################  CLIENTS #################################
# 设置能连上redis的最大客户端连接数量。默认是10000个客户端连接。由于redis不区分连接是客户端连接还
#是内部打开文件或者和slave连接等，所以maxclients最小建议设置到32。如果超过了maxclients，redis会给
#新的连接发送’max number of clients reached’，并关闭连接
# maxclients 10000







#######################    MEMORY MANAGEMENT    ##########################
#redis配置的最大内存容量。当内存满了，需要配合maxmemory-policy策略进行处理。注意slave的输出缓冲区
#是不计算在maxmemory内的。所以为了防止主机内存使用完，建议设置的maxmemory需要更小一些
maxmemory 122000000
#内存容量超过maxmemory后的处理策略。
#volatile-lru：利用LRU算法移除设置过过期时间的key。
#volatile-random：随机移除设置过过期时间的key。
#volatile-ttl：移除即将过期的key，根据最近过期时间来删除（辅以TTL）
#allkeys-lru：利用LRU算法移除任何key。
#allkeys-random：随机移除任何key。
#noeviction：不移除任何key，只是返回一个写错误。
#上面的这些驱逐策略，如果redis没有合适的key驱逐，对于写命令，还是会返回错误。redis将不再接收写请求，只接收get请求。写命令包括：set setnx setex append incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby getset mset msetnx exec sort。
# maxmemory-policy noeviction
# lru检测的样本数。使用lru或者ttl淘汰算法，从需要淘汰的列表中随机选择sample个key，选出闲置时间最长的key移除
# maxmemory-samples 5
# 是否开启salve的最大内存
# replica-ignore-maxmemory yes







##########################    LAZY FREEING    #############################
#以非阻塞方式释放内存
#使用以下配置指令调用了
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no





########################   APPEND ONLY MODE    ###########################


#Redis 默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个写
#操作，并追加到文件中。Redis 重启的会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作
#默认redis使用的是rdb方式持久化，这种方式在许多应用中已经足够用了。但是redis如果中途宕机，会导致可
#能有几分钟的数据丢失，根据save来策略进行持久化，Append Only File是另一种持久化方式，可以提供更好的
#持久化特性。Redis会把每次写入的数据在接收后都写入 appendonly.aof 文件，每次启动时Redis都会先把这
#个文件的数据读入内存里，先忽略RDB文件。若开启rdb则将no改为yes
appendonly no

#指定本地数据库文件名，默认值为 appendonly.aof
appendfilename "appendonly.aof"


#aof持久化策略的配置
#no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快
#always表示每次写入都执行fsync，以保证数据同步到磁盘
#everysec表示每秒执行一次fsync，可能会导致丢失这1s数据
# appendfsync always
appendfsync everysec
# appendfsync no

# 在aof重写或者写入rdb文件的时候，会执行大量IO，此时对于everysec和always的aof模式来说，执行
#fsync会造成阻塞过长时间，no-appendfsync-on-rewrite字段设置为默认设置为no。如果对延迟要求很高的
#应用，这个字段可以设置为yes，否则还是设置为no，这样对持久化特性来说这是更安全的选择。设置为yes表
#示rewrite期间对新写操作不fsync,暂时存在内存中,等rewrite完成后再写入，默认为no，建议yes。Linux的
#默认fsync策略是30秒。可能丢失30秒数据
no-appendfsync-on-rewrite no

#aof自动重写配置。当目前aof文件大小超过上一次重写的aof文件大小的百分之多少进行重写，即当aof文件
#增长到一定大小的时候Redis能够调用bgrewriteaof对日志文件进行重写。当前AOF文件大小是上次日志重写得
#到AOF文件大小的二倍（设置为100）时，自动启动新的日志重写过程
auto-aof-rewrite-percentage 100

#设置允许重写的最小aof文件大小，避免了达到约定百分比但尺寸仍然很小的情况还要重写
auto-aof-rewrite-min-size 64mb

#aof文件可能在尾部是不完整的，当redis启动的时候，aof文件的数据被载入内存。重启可能发生在redis所
#在的主机操作系统宕机后，尤其在ext4文件系统没有加上data=ordered选项（redis宕机或者异常终止不会造
#成尾部不完整现象。）出现这种现象，可以选择让redis退出，或者导入尽可能多的数据。如果选择的是yes，
#当截断的aof文件被导入的时候，会自动发布一个log给客户端然后load。如果是no，用户必须手动redis-
check-aof修复AOF文件才可以
aof-load-truncated yes

#加载redis时，可以识别AOF文件以“redis”开头。
#字符串并加载带前缀的RDB文件，然后继续加载AOF尾巴
aof-use-rdb-preamble yes









#########################   LUA SCRIPTING     ############################
# 如果达到最大时间限制（毫秒），redis会记个log，然后返回error。当一个脚本超过了最大时限。只有
SCRIPT KILL和SHUTDOWN NOSAVE可以用。第一个可以杀没有调write命令的东西。要是已经调用了write，只能
用第二个命令杀
lua-time-limit 5000




#########################   REDIS CLUSTER     ############################
# 集群开关，默认是不开启集群模式
# cluster-enabled yes

#集群配置文件的名称，每个节点都有一个集群相关的配置文件，持久化保存集群的信息。这个文件并不需要手动
#配置，这个配置文件有Redis生成并更新，每个Redis集群节点需要一个单独的配置文件，请确保与实例运行的系
#统中配置文件名称不冲突
# cluster-config-file nodes-6379.conf

#节点互连超时的阀值。集群节点超时毫秒数
# cluster-node-timeout 15000

#在进行故障转移的时候，全部slave都会请求申请为master，但是有些slave可能与master断开连接一段时间
#了，导致数据过于陈旧，这样的slave不应该被提升为master。该参数就是用来判断slave节点与master断线的时
#间是否过长。判断方法是：
#比较slave断开连接的时间和(node-timeout * slave-validity-factor) + repl-ping-slave-period
#如果节点超时时间为三十秒, 并且slave-validity-factor为10,假设默认的repl-ping-slave-period是10
#秒，即如果超过310秒slave将不会尝试进行故障转移
# cluster-replica-validity-factor 10

# master的slave数量大于该值，slave才能迁移到其他孤立master上，如这个参数若被设为2，那么只有当一
#个主节点拥有2 个可工作的从节点时，它的一个从节点会尝试迁移
# cluster-migration-barrier 1

#默认情况下，集群全部的slot有节点负责，集群状态才为ok，才能提供服务。设置为no，可以在slot没有全
#部分配的时候提供服务。不建议打开该配置，这样会造成分区的时候，小分区的master一直在接受写请求，而
#造成很长时间数据不一致
# cluster-require-full-coverage yes


####################  CLUSTER DOCKER/NAT support  #######################
#*群集公告IP
#*群集公告端口
#*群集公告总线端口
# Example:
#
# cluster-announce-ip 10.1.1.5
# cluster-announce-port 6379
# cluster-announce-bus-port 6380










#############################  SLOW LOG #################################
# slog log是用来记录redis运行中执行比较慢的命令耗时。当命令的执行超过了指定时间，就记录在slow log
#中，slog log保存在内存中，所以没有IO操作。
#执行时间比slowlog-log-slower-than大的请求记录到slowlog里面，单位是微秒，所以1000000就是1秒。注
#意，负数时间会禁用慢查询日志，而0则会强制记录所有命令。
slowlog-log-slower-than 10000

#慢查询日志长度。当一个新的命令被写进日志的时候，最老的那个记录会被删掉。这个长度没有限制。只要有足
#够的内存就行。你可以通过 SLOWLOG RESET 来释放内存
slowlog-max-len 128








########################  LATENCY MONITOR ############################
#延迟监控功能是用来监控redis中执行比较缓慢的一些操作，用LATENCY打印redis实例在跑命令时的耗时图表。
#只记录大于等于下边设置的值的操作。0的话，就是关闭监视。默认延迟监控功能是关闭的，如果你需要打开，也
#可以通过CONFIG SET命令动态设置
latency-monitor-threshold 0











#######################  EVENT NOTIFICATION ###########################
#键空间通知使得客户端可以通过订阅频道或模式，来接收那些以某种方式改动了 Redis 数据集的事件。因为开启键空间通知功能需要消耗一些 CPU #所以在默认配置下，该功能处于关闭状态。
#notify-keyspace-events 的参数可以是以下字符的任意组合，它指定了服务器该发送哪些类型的通知：
##K 键空间通知，所有通知以 __keyspace@__ 为前缀
##E 键事件通知，所有通知以 __keyevent@__ 为前缀
##g DEL 、 EXPIRE 、 RENAME 等类型无关的通用命令的通知
##$ 字符串命令的通知
##l 列表命令的通知
##s 集合命令的通知
##h 哈希命令的通知
##z 有序集合命令的通知
##x 过期事件：每当有过期键被删除时发送
##e 驱逐(evict)事件：每当有键因为 maxmemory 政策而被删除时发送
##A 参数 g$lshzxe 的别名
#输入的参数中至少要有一个 K 或者 E，否则的话，不管其余的参数是什么，都不会有任何 通知被分发。详细使用可以参考http://redis.io/topics/notifications

notify-keyspace-events ""










#######################  ADVANCED CONFIG  ###########################
# 数据量小于等于hash-max-ziplist-entries的用ziplist，大于hash-max-ziplist-entries用hash
hash-max-ziplist-entries 512

# value大小小于等于hash-max-ziplist-value的用ziplist，大于hash-max-ziplist-value用hash
hash-max-ziplist-value 64

#-5:最大大小：64 KB<--不建议用于正常工作负载
#-4:最大大小：32 KB<--不推荐
#-3:最大大小：16 KB<--可能不推荐
#-2:最大大小：8kb<--良好
#-1:最大大小：4kb<--良好
list-max-ziplist-size -2

#0:禁用所有列表压缩
#1：深度1表示“在列表中的1个节点之后才开始压缩，
#从头部或尾部
#所以：【head】->node->node->…->node->【tail】
#[头部]，[尾部]将始终未压缩；内部节点将压缩。
#2:[头部]->[下一步]->节点->节点->…->节点->[上一步]->[尾部]
#2这里的意思是：不要压缩头部或头部->下一个或尾部->上一个或尾部，
#但是压缩它们之间的所有节点。
#3:[头部]->[下一步]->[下一步]->节点->节点->…->节点->[上一步]->[上一步]->[尾部]
list-compress-depth 0

# 数据量小于等于set-max-intset-entries用iniset，大于set-max-intset-entries用set
set-max-intset-entries 512

#数据量小于等于zset-max-ziplist-entries用ziplist，大于zset-max-ziplist-entries用zset
zset-max-ziplist-entries 128

#value大小小于等于zset-max-ziplist-value用ziplist，大于zset-max-ziplist-value用zset
zset-max-ziplist-value 64

#value大小小于等于hll-sparse-max-bytes使用稀疏数据结构（sparse），大于hll-sparse-max-bytes使
#用稠密的数据结构（dense）。一个比16000大的value是几乎没用的，建议的value大概为3000。如果对CPU要
#求不高，对空间要求较高的，建议设置到10000左右
hll-sparse-max-bytes 3000

#宏观节点的最大流/项目的大小。在流数据结构是一个基数
#树节点编码在这项大的多。利用这个配置它是如何可能#大节点配置是单字节和
#最大项目数，这可能包含了在切换到新节点的时候
# appending新的流条目。如果任何以下设置来设置
# ignored极限是零，例如，操作系统，它有可能只是一集
#通过设置限制最大#纪录到最大字节0和最大输入到所需的值
stream-node-max-bytes 4096
stream-node-max-entries 100

#Redis将在每100毫秒时使用1毫秒的CPU时间来对redis的hash表进行重新hash，可以降低内存的使用。当你
#的使用场景中，有非常严格的实时性需要，不能够接受Redis时不时的对请求有2毫秒的延迟的话，把这项配置
#为no。如果没有这么严格的实时性要求，可以设置为yes，以便能够尽可能快的释放内存
activerehashing yes

##对客户端输出缓冲进行限制可以强迫那些不从服务器读取数据的客户端断开连接，用来强制关闭传输缓慢的客户端。
#对于normal client，第一个0表示取消hard limit，第二个0和第三个0表示取消soft limit，normal 
client默认取消限制，因为如果没有寻问，他们是不会接收数据的
client-output-buffer-limit normal 0 0 0

#对于slave client和MONITER client，如果client-output-buffer一旦超过256mb，又或者超过64mb持续
#60秒，那么服务器就会立即断开客户端连接
client-output-buffer-limit replica 256mb 64mb 60

#对于pubsub client，如果client-output-buffer一旦超过32mb，又或者超过8mb持续60秒，那么服务器就
#会立即断开客户端连接
client-output-buffer-limit pubsub 32mb 8mb 60

# 这是客户端查询的缓存极限值大小
# client-query-buffer-limit 1gb

#在redis协议中，批量请求，即表示单个字符串，通常限制为512 MB。但是您可以更改此限制。
# proto-max-bulk-len 512mb

#redis执行任务的频率为1s除以hz
hz 10

#当启用动态赫兹时，实际配置的赫兹将用作作为基线，但实际配置的赫兹值的倍数
#在连接更多客户端后根据需要使用。这样一个闲置的实例将占用很少的CPU时间，而繁忙的实例将反应更灵敏
dynamic-hz yes

#在aof重写的时候，如果打开了aof-rewrite-incremental-fsync开关，系统会每32MB执行一次fsync。这
#对于把文件写入磁盘是有帮助的，可以避免过大的延迟峰值
aof-rewrite-incremental-fsync yes

#在rdb保存的时候，如果打开了rdb-save-incremental-fsync开关，系统会每32MB执行一次fsync。这
#对于把文件写入磁盘是有帮助的，可以避免过大的延迟峰值
rdb-save-incremental-fsync yes









###################### ACTIVE DEFRAGMENTATION  ##########################

# 已启用活动碎片整理
# activedefrag yes
# 启动活动碎片整理的最小碎片浪费量
# active-defrag-ignore-bytes 100mb
# 启动活动碎片整理的最小碎片百分比
# active-defrag-threshold-lower 10
# 我们使用最大努力的最大碎片百分比
# active-defrag-threshold-upper 100
# 以CPU百分比表示的碎片整理的最小工作量
# active-defrag-cycle-min 5
# 在CPU的百分比最大的努力和碎片整理
# active-defrag-cycle-max 75
#将从中处理的set/hash/zset/list字段的最大数目
#主词典扫描
# active-defrag-max-scan-fields 1000
```

> 使用配置文件启动 redis-server

```bash
#加载配置文件 启动redis-server
[root@localhost src]# ./redis-server  ./redis.conf 
13821:C 02 Jun 2021 10:31:37.056 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
13821:C 02 Jun 2021 10:31:37.056 # Redis version=5.0.12, bits=64, commit=00000000, modified=0, pid=13821, just started
13821:C 02 Jun 2021 10:31:37.056 # Configuration loaded

#使用客户端测试连接成功
[root@localhost src]# ./redis-cli 
127.0.0.1:6379> keys *
1) "classroom"
2) "age"
3) "name"
127.0.0.1:6379> 
```

## [2.2 docker安装](https://jshand.gitee.io/#/course/server/redis?id=_22-docker安装)

### [1 设置配置文件](https://jshand.gitee.io/#/course/server/redis?id=_1-设置配置文件)

在宿主机（安装docker 容器的的机器）设置配置文件

在usr下创建两级目录

```bash
[root@localhost src] mkdir -p /usr/docker/redis
[root@localhost src] cp  /opt/redis-5.0.12/src/redis.conf  /usr/docker/redis
```

### [2 安装容器](https://jshand.gitee.io/#/course/server/redis?id=_2-安装容器)

```bash
[root@zhangshun ~]# docker run -d  -p 6379:6379 -v /usr/docker/redis:/usr/local/etc/redis/redis.conf  --name redis redis redis-server /usr/local/etc/redis/redis.conf
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
Digest: sha256:910e36ae1071986dfa8f590edfe7a14bd31a1ba78a51efc9d35a74945599549e
Status: Downloaded newer image for redis:latest
51a5b93b78dcd60f7b3d877a724e9a88afe1cc363541815a6e67c7da740a6900


[root@zhangshun ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
51a5b93b78dc   redis                 "docker-entrypoint.s…"   6 seconds ago   Up 5 seconds   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp                                                  redis
```

### [3. 进入到容器执行 redis-cli 验证](https://jshand.gitee.io/#/course/server/redis?id=_3-进入到容器执行-redis-cli-验证)

```bash
# 通过exec 执行通知交互式方式redis 容器的名字 , 进入容器执行的shell /bin/bash
[root@zhangshun ~]# docker exec -it redis  /bin/bash

#切换目录到 有redis-cli的命令的位置
root@51a5b93b78dc:/data# cd /usr/local/bin/


root@51a5b93b78dc:/usr/local/bin# ls -l
total 27596
-rwxrwxr-x 1 root root      374 May 12 19:06 docker-entrypoint.sh
-rwxr-xr-x 1 root root  2404352 Apr 16  2020 gosu
-rwxr-xr-x 1 root root  6903120 May 12 19:07 redis-benchmark
lrwxrwxrwx 1 root root       12 May 12 19:07 redis-check-aof -> redis-server
lrwxrwxrwx 1 root root       12 May 12 19:07 redis-check-rdb -> redis-server
-rwxr-xr-x 1 root root  6754464 May 12 19:07 redis-cli
lrwxrwxrwx 1 root root       12 May 12 19:07 redis-sentinel -> redis-server
-rwxr-xr-x 1 root root 12183728 May 12 19:07 redis-server


#通过cli 连接服务
root@51a5b93b78dc:/usr/local/bin# ./redis-cli 
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name jshand
OK
127.0.0.1:6379> get name
"jshand"
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> exit
root@51a5b93b78dc:/usr/local/bin# exit
exit
[root@zhangshun ~]# 
```

## [2.3 windows安装](https://jshand.gitee.io/#/course/server/redis?id=_23-windows安装)

Redis官方不提供Windows版本，微软在Redis基础上 自己编译了一套 redis，可以用于window上装，仅测试使用

github仓库， https://github.com/microsoftarchive/redis/releases

直接运行 redis-server.exe 可以打开redis服务供调用

如果需要注册系统服务使用如下命令:

```bash
redis-server --service-install redis.windows-service.conf --loglevel verbose
```

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_134833.png)

# [3.使用](https://jshand.gitee.io/#/course/server/redis?id=_3使用)

## [3.1 常用的客户端命令](https://jshand.gitee.io/#/course/server/redis?id=_31-常用的客户端命令)

- 插入数据
  - set name ljs
- 查询数据
  - get name
- 删除键值
  - del name
- 验证键是否存在
  - exsists name
- 返回所有 key
  - keys *
  - keys mylist*
- 设置一个 key 的过期时间(单位:秒)
  - expire name 10
- 移除给定 key 的过期时间
  - persist name
- 选择数据库
  - select
- 将当前数据库中的 key 转移到其它数据库中
  - move name 1
- 返回值的类型
  - type name
- 测试连接是否存活
  - ping
- 返回当前数据库中 key 的数目
  - dbsize
- 监视--实时转储收到的请求
  - monitor
- 删除当前选择数据库中的所有 key
  - flushdb
- 删除所有数据库中的所有 key
- flushall

## [3.2 Redis数据类型](https://jshand.gitee.io/#/course/server/redis?id=_32-redis数据类型)

### [1. strings 类型及操作](https://jshand.gitee.io/#/course/server/redis?id=_1-strings-类型及操作)

string 是最简单的类型 ，一个 key 对应一个value redis 的 string 可以包含任何数据， 比如 jpg 图片或者序列化的对象，从内部实现来看其实 string 可以看作 byte 数组，最大上限是 1G 字节。

- set
  - set name ljs
- Setnx：如果 key 已经存在，返回 0
  - setnx name ljs
- Setex：指定此键值对应的有效期 ,时间单位为秒
  - setex user 100 ljs
- setrange ：设置指定 key 的 value 值的子字符串 ，从指定的位置开始替换字符
  - setrange name 3 @gmail.com
- mset ：一次设置多个 key 的值
  - mset key1 ljs key2 lkr
- getrange ：获取指定 key 的 value 值的子字符串
  - getrange name 0 6
- mget :一次获取多个 key 的值
  - mget key1 key2 key3
- incr:对 key 的值做加加操作
  - incr age
- incrby :同 incr 类似，加指定值
  - incrby age 5
- decr :对 key 的值做的是减减操作
  - decr age
- decrby :同 decr，减指定值
  - decrby age 5
- append :给指定 key 的字符串值追加 value,返回新字符串值的长度
  - append name @126.com
- strlen :取指定 key 的 value 值的长度
  - strlen name

```bash
127.0.0.1:6379> set class java1
OK
127.0.0.1:6379> get class
"java1"
127.0.0.1:6379> setnx class1 java2
(integer) 1
127.0.0.1:6379> get class1
"java2"
127.0.0.1:6379> setnx class2 java3
(integer) 1
127.0.0.1:6379> get class1
"java2"
127.0.0.1:6379> setnx class1 java3
(integer) 0
127.0.0.1:6379> get class1
"java2"
127.0.0.1:6379> mset c1 v1 c2 v2
OK
127.0.0.1:6379> keys *
1) "class1"
2) "class2"
3) "c1"
4) "mylist"
5) "c2"
6) "name"
7) "class"
127.0.0.1:6379> get c2
"v2"
127.0.0.1:6379> mget c1 c2
1) "v1"
2) "v2"
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> incr age
(integer) 19
127.0.0.1:6379> get age
"19"
127.0.0.1:6379> incrby  age  6
(integer) 25
127.0.0.1:6379> flushall
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> select 2
OK
127.0.0.1:6379[2]> flushdb
OK
127.0.0.1:6379[2]> select 0
OK
127.0.0.1:6379> set name jshand
OK
127.0.0.1:6379> get name
"jshand"
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> get name
(nil)
127.0.0.1:6379[1]> flushdb
OK
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> get ma,e
(nil)
127.0.0.1:6379> get name
"jshand"
127.0.0.1:6379> select 10
OK
127.0.0.1:6379[10]> flushall
OK
127.0.0.1:6379[10]> select 0
OK
127.0.0.1:6379> keys *
(empty array)
```

### [2. hash 类型及操作](https://jshand.gitee.io/#/course/server/redis?id=_2-hash-类型及操作)

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象，相较于将对象的每个字段存成单个 string 类型，将一个对象存储在 hash 类型中会占用更少的内存，并且可以更方便的存取整个对象。

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_115123.png)

- hset :设置 hash field 为指定值
  - hset myhash name ljs
- hget :获取指定的 hash field
  - hget myhash name
- Hmset:同时设置 hash 的多个 field
  - hmset myhash name ljs age 20
- hmget :获取全部指定的 hash filed
  - hmget myhash name age password
- hlen :返回指定 hash 的 field 数量
  - hlen myhash
- hdel
  - hdel myhash field1
- hkeys:返回 hash 的所有 field
  - hkeys myhash
- hvals :返回 hash 的所有 value
  - hvals myhash
- hgetall :获取某个 hash 中全部的 filed 及 value
  - hgetall myhash

```
127.0.0.1:6379> hset zs name zhangsan 
(integer) 1
127.0.0.1:6379> hget zs name
"zhangsan"
127.0.0.1:6379> hset zs age 18 address HLJ
(integer) 2
127.0.0.1:6379> hmget zs name
1) "zhangsan"
127.0.0.1:6379> hmget zs name age address
1) "zhangsan"
2) "18"
3) "HLJ"
127.0.0.1:6379> 
```

### [3. lists 类型及操作](https://jshand.gitee.io/#/course/server/redis?id=_3-lists-类型及操作)

list 是一个链表结构，主要功能是 push、 pop、获取一个范围的所有值等， 操作中 key 理解为链表的名字。 Redis 的 list 类型其实就是一个每个子元素都是 string 类型的双向链表。链表的最大长度是(2的 32 次方)。我们可以通过 push,pop 操作从链表的头部或者尾部添加删除元素。这使得 list既可以用作栈，也可以用作队列。

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_115217.png)

- Lpush:在 key 对应 list 的头部添加字符串元素
  - lpush mylist world lpush mylist hello
- lrange
  - lrange mylist 0 1
- rpush :在 key 对应 list 的尾部添加字符串元素
  - rpush mylist1 world rpush mylist1 hello
- linsert :在 key 对应 list 的特定位置之前或之后添加字符串元素
  - linsert mylist 1 before world there
- lset :设置 list 中指定下标的元素值(下标从 0 开始)
  - rpush mylist2 one
  - rpush mylist2 two
  - rpush mylist2 three
  - lset mylist2 0 four
- lrem :从 key 对应 list 中删除 count 个和 value 相同的元素
  - rpush mylist3 hello
  - rpush mylist3 hello
  - rpush mylist3 world
  - rpush mylist3 hello
  - rpush mylist3 hello
  - lrem mylist3 2 hello
- ltrim :保留指定 key 的值范围内的数据
  - rpush mylist4 1
  - rpush mylist4 2
  - rpush mylist4 3
  - rpush mylist4 4
  - rpush mylist4 5
  - ltrim mylist4 1 3
- lpop :从 list 的头部删除元素，并返回删除元素
  - lpop mylist4
- rpop :从 list 的尾部删除元素，并返回删除元素
  - rpop mylist4
- lindex :返回名称为 key 的 list 中 index 位置的元素
  - lindex mylist4 0
- llen :返回 key 对应 list 的长度
  - llen mylist4

```bash
127.0.0.1:6379> lpush stus zhangsan lisi
(integer) 2
127.0.0.1:6379> get stus
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> lrange stus 0 2
1) "lisi"
2) "zhangsan"
127.0.0.1:6379> flushall
OK
127.0.0.1:6379> rpush stus zs li ww
(integer) 3
127.0.0.1:6379> lrange stus 0 5
1) "zs"
2) "li"
3) "ww"
```

### [4. sets 类型及操作](https://jshand.gitee.io/#/course/server/redis?id=_4-sets-类型及操作)

Redis 的 set 是 string 类型的无序集合。 set 元素最大可以包含(2 的 32 次方)个元素。 可以对集合进行添加删除元素，通过key对多个集合求交并差等操作， 操作中 key 理解为集合的名字。 set 是通过 hash table 实现的， hash table 会随着添加或者删除自动的调整大小。

- sadd
  - sadd myset hello
  - sadd myset world
  - sadd myset hello
- smembers ：查看 myset 中的所有元素
  - smembers myset
- srem ：删除名称为 key 的 set 中的元素 member
  - srem myset hello
- spop ：随机删除名称为 key 的 set 中一个元素
  - spop myset
- Sdiff:返回第一个 set集合与第二个 set集合的差集
- sadd myset1 one
  - sadd myset1 two
  - sadd myset1 three
  - sadd myset2 two
  - sadd myset2 three
  - sadd myset2 four
  - sdiff myset2 myset1
- sdiffstore :返回第一个 set集合与第二个 set集合的差集 ，并将结果存为另一个 set
  - sdiffstore myset3 myset2 myset1
- Sinter:返回所有给定 key 的交集 （获取共同的朋友）
  - sinter myset1 myset2
- sinterstore :返回所有给定 key 的交集，并将结果存为另一个 key
  - sinterstore myset4 myset2 myset1
- Sunion:返回所有给定 key 的并集
  - sunion myset2 myset1
- sunionstore :返回所有给定 key 的并集，并将结果存为另一个 key
  - sunionstore myset5 myset2 myset1
- Scard:返回名称为 key 的 set 的元素个数
  - scard myset2

### [5. sorted sets类型及操作](https://jshand.gitee.io/#/course/server/redis?id=_5-sorted-sets类型及操作)

sorted set 是 set 的一个升级版本， 它在 set 的基础上增加了一个分数属性，使我们能够获得分数最高、最低的前n个元素，获得指定分数范围内的元素等与分数有关的操作。 和 set 一样 ，sorted set 也是 string 类型元素的集合，不同的是每个元素都会关联一个 double类型的 score。

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_143601.png)

- zadd：向名称为 key 的 zset 中添加元素 member， score 用于设定分数。如果该元素已经存在，则根据score 更新该元素的分数。
  - zadd key score member
  - zadd myzset 1 one
  - zadd myzset 2 two
  - zadd myzset 3 two
  - zrange myzset 0 -1 withscores
- zrem :删除名称为 key 的 zset 中的元素 member
  - zrem myzset two
- zrevrank：返回zset 中名称为 key 的 member 元素的排名(按 score 从大到小排序)即下标
  - zrevrank myzset1 two
- zrevrange ：按 score 从大到小排序，再取出全部元素
  - zrevrange myzset1 0 -1 withscores
- zrangebyscore ：返回集合中 score 在给定区间的元素
  - zrangebyscore myzset1 2 3 withscores
- zcount ：返回集合中 score 在给定区间的数量
  - zcount myzset1 2 3
- zcard ：返回集合中元素个数
  - zcard myzset1
- zscore ：返回给定元素对应的 score
  - zscore myzset1 two
- zremrangebyrank ：删除集合中排名在给定区间的元素
  - zremrangebyrank myzset1 3 3
- zremrangebyscore ：删除集合中 score 在给定区间的元素
  - zremrangebyscore myzset1 1 2

# [4.Jedits客户端](https://jshand.gitee.io/#/course/server/redis?id=_4jedits客户端)

Redis官方提供了很多客户端，如下图所示

https://redis.io/clients

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_152051.png)

Java语言的客户端如下：

![2021-06-02_152107](https://jshand.gitee.io/imgs/redis/2021-06-02_152107.png)

https://mvnrepository.com/artifact/redis.clients/jedis

此处采用 Jedis连接操作

- 添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.neuedu</groupId>
    <artifactId>redis-java</artifactId>
    <version>1.0-SNAPSHOT</version>


    <dependencies>
        <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.3.0</version>
        </dependency>



        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>teset</scope>
        </dependency>
    </dependencies>

</project>
```

- 创建连接

```java
 Jedis jedis = new Jedis("localhost");
```

- 使用jedis客户端

```java
package com.neuedu.jedis;

import org.junit.Test;
import redis.clients.jedis.Jedis;

/**
 * 项目 redis-java
 *
 * @author 张金山
 * @version 1.0
 * 说明 Jedis客户端
 * @date 2021/6/2 15:33
 */
public class JedisClient {


    @Test
    public void test1(){

        String  host = "localhost";
        Jedis jedis = new Jedis(host);

        //测试服务器的联通性
        String msg = jedis.ping();
        System.out.println("连接到服务器，ping 消息 :"+msg);

        //向redis中添加一个hash
        jedis.hset("user","name","zhangsan");
    }
}
```

通过客户端软件查看刚才添加的key

![img](https://jshand.gitee.io/imgs/redis/2021-06-02_154235.png)

# [5.SpringBoot集成Redis](https://jshand.gitee.io/#/course/server/redis?id=_5springboot集成redis)

## [5.1 添加依赖](https://jshand.gitee.io/#/course/server/redis?id=_51-添加依赖)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.4</version>
    <relativePath/>
  </parent>

  <groupId>com.neuedu</groupId>
  <artifactId>data-redis-springboot</artifactId>
  <version>1.0-SNAPSHOT</version>

  <packaging>jar</packaging>
  <name>data-redis-springboot</name>


  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <!--使用Jedis客户端 -->
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
    </dependency>
  </dependencies>


</project>
```

## [5.2 配置Redis客户端](https://jshand.gitee.io/#/course/server/redis?id=_52-配置redis客户端)

```properties
# Redis数据库索引（默认为0）  
spring.redis.database=0  

# Redis服务器地址  
spring.redis.host=127.0.0.1  

# Redis服务器连接端口  
spring.redis.port=6379  

# Redis服务器连接密码（默认为空）  
spring.redis.password=  

# 连接池最大连接数（使用负值表示没有限制）  
spring.redis.pool.max-active=200  

# 连接池最大阻塞等待时间（使用负值表示没有限制）  
spring.redis.pool.max-wait=-1  

# 连接池中的最大空闲连接  
spring.redis.pool.max-idle=10 

# 连接池中的最小空闲连接  
spring.redis.pool.min-idle=0 

# 连接超时时间（毫秒）  
spring.redis.timeout=1000 
```

application.yml

```yaml
server:
  port: 80

spring:
  #配置Redis客户端
  redis:
    # Redis数据库索引（默认为0）
    database: 0
    # Redis服务器地址
    host: 127.0.0.1
    # Redis服务器连接端口
    port: 6379
```

在controller中使用

```java
package com.neuedu;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * 项目 data-redis-springboot
 *
 * @author 张金山
 * @version 1.0
 * 说明 Redis测试
 * @date 2021/6/2 16:00
 */
@RestController
@RequestMapping("/redis")
public class RedisController {


    @Resource
    RedisTemplate redisTemplate;


    /**
     * http://127.0.0.1/redis/hello
     * @return
     */
    @RequestMapping("/set")
    public String set() {

        redisTemplate.opsForValue().set("user","张三");

        return "欢迎访问 redis-data";
    }


    @RequestMapping("/get")
    public String get() {

        String user = (String) redisTemplate.opsForValue().get("user");

        return user;
    }


}
```

自定义RedisTesmplate使用String的方式序列化

```java
/**
 * 项目 data-redis-springboot
 *
 * @author 张金山
 * @version 1.0
 * 说明 配置类型
 * @date 2021/6/3 8:45
 */
@Configuration
public class RedisConfig {


    @Primary
    @Bean
    public RedisTemplate getRedisTempate(RedisTemplate redisTemplate){
        RedisSerializer redisSerializer = new StringRedisSerializer();

        redisTemplate.setKeySerializer(redisSerializer);
        redisTemplate.setValueSerializer(redisSerializer);


        redisTemplate.setHashValueSerializer(redisSerializer);
        redisTemplate.setHashValueSerializer(redisSerializer);

        return redisTemplate;
    }

}
```

## [5.3 RedisTemplate封装](https://jshand.gitee.io/#/course/server/redis?id=_53-redistemplate封装)

针对RedisTemplate进行封装，使操作更简单，创建RedisUtil工具类，并注入RedisTempate对象

```java
package com.neuedu;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.util.CollectionUtils;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

/**
 * 项目 data-redis-springboot
 *
 * @author 张金山
 * @version 1.0
 * 说明 Redis测试
 * @date 2021/6/3 09:05
 */
public class RedisUtil {

    private RedisTemplate<String, String> redisTemplate;

    public RedisUtil(RedisTemplate<String, String> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }





    //=============================common============================

    /**
     * 指定缓存失效时间
     *
     * @param key  键
     * @param time 时间(秒)
     * @return
     */
    public boolean expire(String key, long time) {
        try {
            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     *
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }

    /**
     * 判断key是否存在
     *
     * @param key 键
     * @return true 存在 false不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 删除缓存
     *
     * @param key 可以传一个值 或多个
     */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete((Collection<String>) CollectionUtils.arrayToList(key));
            }
        }
    }

    //============================String=============================  

    /**
     * 普通缓存获取
     *
     * @param key 键
     * @return 值
     */
    public String get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /**
     * 普通缓存放入
     *
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */
    public boolean set(String key, String value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }

    /**
     * 普通缓存放入并设置时间
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */
    public boolean set(String key, String value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 递增
     *
     * @param key 键
     * @param delta  要增加几(大于0)
     * @return
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }

    /**
     * 递减
     *
     * @param key 键
     * @param delta  要减少几(小于0)
     * @return
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }

    //================================Map=================================  

    /**
     * HashGet
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return 值
     */
    public String hget(String key, String item) {
        return (String) redisTemplate.opsForHash().get(key, item);
    }

    /**
     * 获取hashKey对应的所有键值
     *
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     *
     * @param key 键
     * @param map 对应多个键值
     * @return true 成功 false 失败
     */
    public boolean hmset(String key, Map<String, String> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * HashSet 并设置时间
     *
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, String> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, String value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒)  注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, String value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, String... item) {
        redisTemplate.opsForHash().delete(key, item);
    }

    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }

    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     * @return
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }

    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     * @return
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }

    //============================set=============================  

    /**
     * 根据key获取Set中的所有值
     *
     * @param key 键
     * @return
     */
    public Set<String> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, String value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, String... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSetAndTime(String key, long time, String... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0) {
                expire(key, time);
            }
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 获取set缓存的长度
     *
     * @param key 键
     * @return
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */
    public long setRemove(String key, String... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }
    //===============================list=================================  

    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束  0 到 -1代表所有值
     * @return
     */
    public List<String> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 获取list缓存的长度
     *
     * @param key 键
     * @return
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引  index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     * @return
     */
    public String lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, String value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, String value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, List<String> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, List<String> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */
    public boolean lUpdateIndex(String key, long index, String value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */
    public long lRemove(String key, long count, String value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }
}
```

在容器中声明Bean（RedisUtil）

```java
package com.neuedu.config;

import com.neuedu.RedisUtil;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import javax.annotation.PostConstruct;

/**
 * 项目 data-redis-springboot
 *
 * @author 张金山
 * @version 1.0
 * 说明 配置类型
 * @date 2021/6/3 8:45
 */
@Configuration
public class RedisConfig {


    /**
     * 修改RedisTemplate 序列化方式
     * @param redisTemplate
     * @return
     */
    @Primary
    @Bean
    public RedisTemplate getRedisTempate(RedisTemplate redisTemplate){
        RedisSerializer redisSerializer = new StringRedisSerializer();

        redisTemplate.setKeySerializer(redisSerializer);
        redisTemplate.setValueSerializer(redisSerializer);


        redisTemplate.setHashValueSerializer(redisSerializer);
        redisTemplate.setHashValueSerializer(redisSerializer);

        return redisTemplate;
    }

    /**
     * 早容器中声明一个 RedisUtil
     * @param redisTemplate
     * @return
     */
    @Bean
    public RedisUtil getRedisUtil(RedisTemplate redisTemplate){

        return new RedisUtil(redisTemplate);
    }


}
```

在业务中使用RedisUtil(在Controller中使用)

```java
package com.neuedu;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * 项目 data-redis-springboot
 *
 * @author 张金山
 * @version 1.0
 * 说明 Redis测试
 * @date 2021/6/2 16:00
 */
@RestController
@RequestMapping("/redis")
public class RedisController {


    @Resource
    RedisTemplate redisTemplate;


    @Resource
    RedisUtil redisUtil;

    /**
     * http://127.0.0.1/redis/hello
     * @return
     */
    @RequestMapping("/set")
    public String set() {

        redisTemplate.opsForValue().set("user","张三");
        return "欢迎访问 redis-data";
    }


    @RequestMapping("/get")
    public String get() {
        String user = (String) redisTemplate.opsForValue().get("user");
        return user;
    }




    /**
     * http://127.0.0.1/redis/uset
     * @return
     */
    @RequestMapping("/uset")
    public String uset() {

        redisUtil.set("utilKey","utilValue");
        return "欢迎访问 redis-data";
    }


    /**
     * http://127.0.0.1/redis/uget
     * @return
     */
    @RequestMapping("/uget")
    public String uget() {
        String user = redisUtil.get("utilKey");
        return user;
    }
}
```

# [6.案例](https://jshand.gitee.io/#/course/server/redis?id=_6案例)

前端项目地址：

后端项目地址：https://gitee.com/harbin-university-18-java1/redis-ssm-server.git

> 需求，用户登录案例需要记录最后一次操作时间如果超过30分钟，则提示用户重新登录

## [6.1账户表](https://jshand.gitee.io/#/course/server/redis?id=_61账户表)

 创建数据库的物理结构

![img](https://jshand.gitee.io/imgs/redis/2021-06-03_094552.png)

```sql
/*
SQLyog Ultimate v12.08 (64 bit)
MySQL - 5.7.18 : Database - redis
*********************************************************************
*/


/*!40101 SET NAMES utf8 */;

/*!40101 SET SQL_MODE=''*/;

/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
CREATE DATABASE /*!32312 IF NOT EXISTS*/`redis` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;

USE `redis`;

/*Table structure for table `ums_user` */

DROP TABLE IF EXISTS `ums_user`;

CREATE TABLE `ums_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(100) DEFAULT NULL COMMENT '用户名',
  `password` varchar(100) DEFAULT NULL COMMENT '密码',
  `nickname` varchar(500) DEFAULT NULL COMMENT '昵称',
  `lastlogin` datetime DEFAULT NULL COMMENT '最后一次登录时间',
  `active` varchar(10) DEFAULT '1' COMMENT '1 有效，0 无效',
  `uptime` datetime DEFAULT NULL,
  `ctime` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户账户表';

/*Data for the table `ums_user` */

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
```

## [6.2 搭建前后端分离的项目](https://jshand.gitee.io/#/course/server/redis?id=_62-搭建前后端分离的项目)

后端：ssm（MyBatisPlus）

前端：Vuejs（axios+elementui+vuex）

## [6.3 后端项目](https://jshand.gitee.io/#/course/server/redis?id=_63-后端项目)

### [1. SpringBoot-web支持](https://jshand.gitee.io/#/course/server/redis?id=_1-springboot-web支持)

### [2.MyBatisPlus支持](https://jshand.gitee.io/#/course/server/redis?id=_2mybatisplus支持)

- 添加MyBatisPlus的
- 添加逆向工程

> 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>8</source>
          <target>8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.4</version>
    <relativePath/>
  </parent>

  <groupId>com.neuedu</groupId>
  <artifactId>data-redis-springboot</artifactId>
  <version>1.0-SNAPSHOT</version>

  <packaging>jar</packaging>
  <name>data-redis-springboot</name>


  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
    </dependency>

    <!--使用Jedis客户端 -->
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
    </dependency>


    <!--热部署的工具 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
    </dependency>

    <!-- apache 的工具类 -->
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
      <!--            <version>3.3.2</version>-->
    </dependency>

    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.3.2</version>
    </dependency>

    <!-- MySQL驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
    </dependency>


    <!-- mybatis-plus 代码生成器-->
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-generator</artifactId>
      <version>3.3.2</version>
      <scope>test</scope>
    </dependency>


    <!-- freemarker 模板引擎-->
    <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
      <version>2.3.31</version>
      <scope>test</scope>
    </dependency>


    <!-- japidocs 生成HTML、PDF等格式的  接口文档 -->
    <dependency>
      <groupId>io.github.yedaxia</groupId>
      <artifactId>japidocs</artifactId>
      <version>1.4.3</version>
      <scope>test</scope>
    </dependency>



  </dependencies>


</project>
```

### [3. 代码自动生成](https://jshand.gitee.io/#/course/server/redis?id=_3-代码自动生成)

```java
package com.neuedu.mp;

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class AutoCode {


    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        final String projectPath = System.getProperty("user.dir");


        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("金山");
        gc.setOpen(false);
        // gc.setSwagger2(true); 实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/redis?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
//        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.neuedu.redis");
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

        // 如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/com/neuedu/redis/mapper/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录，自定义目录用");
                if (fileType == FileType.MAPPER) {
                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false
                    return !new File(filePath).exists();
                }
                // 允许生成模板文件
                return true;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
//        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);
        // 公共父类
//        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
//        strategy.setSuperEntityColumns("id");
//        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
//        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

}
```

### [4. 添加数据源](https://jshand.gitee.io/#/course/server/redis?id=_4-添加数据源)

```yaml
server:
  port: 80

logging:
  level:
    com.neuedu: DEBUG
    org: INFO

spring:
  mvc:
    throw-exception-if-no-handler-found: true
  web:
    resources:
      add-mappings: false
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/redis?useUnicode=true&useSSL=false&characterEncoding=utf8
    username: root
    password: root
  #配置Redis客户端
  redis:
    # Redis数据库索引（默认为0）
    database: 0
    # Redis服务器地址
    host: 127.0.0.1
    # Redis服务器连接端口
    port: 6379
```

### [5.测试MyBatisPlus环境是否可用](https://jshand.gitee.io/#/course/server/redis?id=_5测试mybatisplus环境是否可用)

```java
package com.neuedu.redis.service;

import com.neuedu.redis.App;
import com.neuedu.redis.entity.UmsUser;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.time.LocalDateTime;

import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest(classes = App.class)
class IUmsUserServiceTest {

    @Autowired
    IUmsUserService umsUserService;


    @Test
    public void testInsert(){

        UmsUser umsUser = new UmsUser();
        umsUser.setUsername("admin");
        umsUser.setPassword("123456");
        umsUser.setLastlogin(LocalDateTime.now());
        boolean ret = umsUserService.save(umsUser);
    }

}
```

## [6.4 前端项目](https://jshand.gitee.io/#/course/server/redis?id=_64-前端项目)

```bash
vue create redis-front
```

启动测试

```
E:\development\java18_1>cd redis-front

E:\development\java18_1\redis-front>npm run serve

> redis-front@0.1.0 serve
> vue-cli-service serve

 INFO  Starting development server...
```

### [1. 修改配置项](https://jshand.gitee.io/#/course/server/redis?id=_1-修改配置项)

 开发的 默认端口改为 80 ,去掉Eslint校验

 编辑项目根目录中的 vue.config.js

```js
module.exports = {
    lintOnSave: false,
    devServer: {
        port: 80 //修改启动端口
    }
}
```

### [2. 搭建增删改查环境](https://jshand.gitee.io/#/course/server/redis?id=_2-搭建增删改查环境)

添加依赖axios、element-ui、router、vuex、vuex-persistedstate等

```shell
npm i element-ui -S
npm install --save axios vue-router vuex vuex-persistedstate 
```

### [3. 封装axios](https://jshand.gitee.io/#/course/server/redis?id=_3-封装axios)

新建一个 util/request.js

```js
import axios from 'axios'
// import store from '@/vuex'


axios.defaults.baseURL = 'http://127.0.0.1:8080'
// if (store.getters.getToken) {
//   axios.defaults.headers.common['token'] = store.getters.getToken
// }


import {
    Loading,
    Message,
    MessageBox
} from 'element-ui'
export default {
    get(url, callback, params = {}) {
        const loading = Loading.service({
            lock: true,
            text: '数据加载中',
            spinner: 'el-icon-loading',
            background: 'rgba(255, 255, 255, 0.7)'
        })
        axios.get(url, {
            params: params
        }).then(response => {
            if (response.data.code === 200) {
                callback(response.data)
            } else {
                Message.error(response.data.message)
            }
        }).catch(err => {
            Message.error(err)
        }).finally(() => {
            loading.close()
        })
    },
    post(url, callback, params = {}, msg) {
        const formData = new FormData()
        for (let key in params) {
            formData.append(key, params[key])
        }
        const loading = Loading.service({
            lock: true,
            text: '数据提交中',
            spinner: 'el-icon-loading',
            background: 'rgba(255, 255, 255, 0.7)'
        })
        setTimeout(() => {
            loading.close()
        }, 10000)
        axios.post(url, formData)
            .then(response => {
                if (response.data.code === 200) {

                    if (msg === undefined) {
                        Message.success(response.data.message)
                    } else if (msg != null && msg.length != 0) {
                        Message.success(msg)
                    }

                    callback(response.data)
                } else {
                    Message.error(response.data.message)
                }
            }).catch(err => {
                Message.error(err)
            }).finally(() => {
                loading.close()
            })

    }
    // setToken(token) {
    //     axios.defaults.headers.common['token'] = token
    // }
}
```

在main.js中应用，在VUE原型添加 axios对象(get\post)

```js
import Vue from 'vue'

import App from './App.vue'


import axios from './util/request.js'

import Router from 'vue-router'

Vue.config.productionTip = false


Vue.prototype.axios = axios
Vue.prototype.getReq = axios.get

new Vue({
  render: h => h(App),
}).$mount('#app')
```

### [4.封装router](https://jshand.gitee.io/#/course/server/redis?id=_4封装router)

util/request.js

```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'Index',
      component: () => import('@/views'),
      children: [
        {
          path: '/user',
          name: 'User',
          component: () => import('@/views/user')
        }
      ]
    },
    {
      path: '/login',
      name: 'Login',
      component: () => import('@/views/login')
    }
  ]
})
```

在main.js中添加路由

```js
import Vue from 'vue'

import App from './App.vue'


import axios from './util/request.js'

import router from './util/router.js'

Vue.config.productionTip = false


Vue.prototype.axios = axios
Vue.prototype.getReq = axios.get

new Vue({
    router,
    render: h => h(App),
}).$mount('#app')
```

### [5. 在App.vue中添加路由组件](https://jshand.gitee.io/#/course/server/redis?id=_5-在appvue中添加路由组件)

```vue
<template>
  <router-view></router-view>
</template>

<script>

export default {
  name: 'App',
  components: {

  }
}
</script>

<style scoped>

</style>
```

### [6.测试路由](https://jshand.gitee.io/#/course/server/redis?id=_6测试路由)

```
http://localhost/login
http://localhost/
```

### [7.引用Element-UI组件](https://jshand.gitee.io/#/course/server/redis?id=_7引用element-ui组件)

在main.js中添加引用 组件、样式

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

### [8.添加欢迎页面](https://jshand.gitee.io/#/course/server/redis?id=_8添加欢迎页面)

在App.vue中重置了一下全局的 样式

```html
<style >
    *{
        margin: 0;
        padding: 0;
    }
</style>
```

@/views/index.vue的内容如下

```vue
<template>


    <el-container class="container" >
      <el-header class="top" >Header</el-header>
      <el-container>
        <el-aside width="280px" class="left" >Aside</el-aside>
        <el-main class="main">Main</el-main>
      </el-container>
    </el-container>


</template>

<script>
    export default {
        name:'Index',
        data(){
            return {
                name:'首页'
            }
        }
    }
</script>

<style scoped>

    .container{
        border: 1px solid #00FFFF;
        height: 100vh;
    }



    .top{
        background-color: #3A8EE6;
    }


    .left{
        background-color: #606266;
    }

</style>
```

### [9.登录画面](https://jshand.gitee.io/#/course/server/redis?id=_9登录画面)

```
<template>


    <div class="wrapContainer">
        <div class="container">

            <el-form ref="form" :model="form">
                <el-form-item>
                    <el-input v-model="form.username" placeholder="用户名"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-input v-model="form.password" type="password" placeholder="密码"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary">登录</el-button>

                </el-form-item>
            </el-form>




        </div>
    </div>





</template>

<script>
    export default {
        name: 'Login',
        data() {
            return {
                form:{
                    username: '',
                    password: ''
                }
            }
        }
    }
</script>

<style scoped>
    .wrapContainer {
        width: 100%;
        height: 100vh;
        /* border: solid 1px  black; */
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .container {
        width: 400px;
        height: 300px;
        /* border: 1px solid green; */
        text-align: center;
    }
</style>
```

## [6.5 开发用户表单CRUD功能](https://jshand.gitee.io/#/course/server/redis?id=_65-开发用户表单crud功能)

### [1. 封装菜单](https://jshand.gitee.io/#/course/server/redis?id=_1-封装菜单)

### [2.用户的列表页](https://jshand.gitee.io/#/course/server/redis?id=_2用户的列表页)

### [3.开发查询接口](https://jshand.gitee.io/#/course/server/redis?id=_3开发查询接口)

- 分页插件

```java
 /**
     * MyBatisPlus 分页插件
     * @return
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;
    }
```

- 分页方法

```java
package com.neuedu.redis.controller;


import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.neuedu.redis.entity.UmsUser;
import com.neuedu.redis.service.IUmsUserService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * <p>
 * 用户账户表 前端控制器
 * </p>
 *
 * @author 金山
 * @since 2021-06-03
 */
@Slf4j
@RestController
@RequestMapping("/umsUser")
public class UmsUserController {

    @Resource
    IUmsUserService umsUserService;

    /**
     * 1 分页查询
     * http://127.0.0.1:8080/umsUser/list
     * @param page
     * @return
     */
    @GetMapping("/list")
    public Page<UmsUser>  list(Page page){
        Page result = umsUserService.page(page);

        log.info("当前页号:{}",page.getCurrent());
        log.info("总页数:{}",page.getPages());
        log.info("总条数:{}",page.getTotal());

        return result;
    }






}
```

### [4.封装统一的返回结果](https://jshand.gitee.io/#/course/server/redis?id=_4封装统一的返回结果)

### [5.解决跨域的问题(SpringBoot解决跨域)](https://jshand.gitee.io/#/course/server/redis?id=_5解决跨域的问题springboot解决跨域)

```java
    @Bean
    CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.addAllowedOrigin("*");
        configuration.addAllowedMethod("*");
        configuration.addAllowedHeader("*");
        source.registerCorsConfiguration("/**", configuration);
        return new CorsFilter(source);
    }
```

### [6. 列表页调用axios获取列表](https://jshand.gitee.io/#/course/server/redis?id=_6-列表页调用axios获取列表)

## [6.6. 登录案例](https://jshand.gitee.io/#/course/server/redis?id=_66-登录案例)

验证的逻辑

- 拿着用户名 现在表中查询
- 如果用户名不存在 返回 用户名或密码错误
- 如果用户名存在 通过用户名 查询数据库的数据
- 然后再进行密码校验 如果密码校验也成功说明可以登录(返回Token),保存到缓存中(key-value)(username:token ,最后一次操作时间)
- 如果密码校验不成功 还是用户名或密码错误

登录逻辑如下:

![img](https://jshand.gitee.io/imgs/redis/2021-06-04_135933.png)

### [1.后端添加jwt依赖](https://jshand.gitee.io/#/course/server/redis?id=_1后端添加jwt依赖)

### [2.添加spring-security-core](https://jshand.gitee.io/#/course/server/redis?id=_2添加spring-security-core)

### [3.生成token](https://jshand.gitee.io/#/course/server/redis?id=_3生成token)

### [4.添加拦截器验证是否存在token请求](https://jshand.gitee.io/#/course/server/redis?id=_4添加拦截器验证是否存在token请求)

授权认证

- 解析出jwt 拿到username
- 通过username 到redis中 查看 key是否存在
- 如果存在 则从redis中 取出user对象 和 jwt中 进行密码和时间校验
- 如果都对 则 可以通过 说明有认证
- 每次请求前端需要带着token(在请求header中传),
- 从header中获取token,跟redis中的比较如果一致，
  - 未超时: 200放行
  - 超时：401超时
- token不一致否则403未登录

# [7. ssdb](https://jshand.gitee.io/#/course/server/redis?id=_7-ssdb)

## [7.1 介绍](https://jshand.gitee.io/#/course/server/redis?id=_71-介绍)

SSDB 是一个 C/C++ 语言开发的高性能 NoSQL 数据库, 支持 zset(sorted set), map(hash), kv, list 等数据结构, 用来替代或者与 Redis 配合存储十亿级别列表的数据. SSDB 在 QIHU 360 被大量使用, 同时也被国内外业界的[众多互联网企业所使用](http://ssdb.io/#users).

> 官方介绍：一个高性能的支持丰富数据结构的 NoSQL 数据库, 用于替代 Redis.

特性

- 替代 Redis 数据库, Redis 的 100 倍容量
- LevelDB 网络支持, 使用 C/C++ 开发
- Redis API 兼容, 支持 Redis 客户端
- 适合存储集合数据, 如 list, hash, zset...
- 客户端 API 支持的语言包括: [C++](http://ssdb.io/docs/cpp/), [PHP](http://ssdb.io/docs/zh_cn/php/), Python, [Java](http://ssdb.io/docs/java/), Go
- 持久化的队列服务
- 主从复制, 负载均衡

## [7.2 linux下安装](https://jshand.gitee.io/#/course/server/redis?id=_72-linux下安装)

```bash
wget --no-check-certificate https://github.com/ideawu/ssdb/archive/master.zip
unzip master
cd ssdb-master
make
# optional, install ssdb in /usr/local/ssdb
sudo make install
```

## [7.3 linux 启动](https://jshand.gitee.io/#/course/server/redis?id=_73-linux-启动)

```bash
# start master
./ssdb-server ssdb.conf

# or start as daemon
./ssdb-server -d ssdb.conf
```

## [7.2 docker下安装 ssdb](https://jshand.gitee.io/#/course/server/redis?id=_72-docker下安装-ssdb)

```bash
#下载容器
docker run -p 6381:8888 --name ssdbServer -dit centos:7.7.1908 /bin/bash

#进入到容器
docker exec -it ssdbServer /bin/bash

#在容器中安装工具包
yum install -y wget unzip gcc gcc-c++ automake autoconf libtool make which

#下载ssdb源码
wget --no-check-certificate https://github.com/ideawu/ssdb/archive/master.zip

#解压ssdb源码
unzip master

#进入到snappy
cd /ssdb-master/deps/snappy-1.1.0

#安装 snappy
./configure  && make

#回到 ssdb源码包
cd ../..

#编译、安装
make && make install

#为了后面在容器中使用，在容器中创建一个目录，后期为了映射ssdb配置文件
mkdir /ssdb

#暂时将配置文件copy到ssdb中
cp ssdb.conf /ssdb

#使用临时配置文件启动 ssdb-server 并变成守护进程
./ssdb-server -d /ssdb/ssdb.conf 
```

倒数第二步骤中复制完配置文件 需要修改配置文件中的相对目录

> work_dir = /ssdb-master/var
>
> pidfile = /ssdb-master/var/ssdb.pid

```bash
# ssdb-server config
# MUST indent by TAB!

# absolute path, or relative to path of this file, directory must exists
work_dir = /ssdb-master/var

pidfile = /ssdb-master/var/ssdb.pid

server:
    # specify an ipv6 address to enable ipv6 support
    # ip: ::1
    ip: 127.0.0.1
    port: 8888
    # bind to public ip
    #ip: 0.0.0.0
    # format: allow|deny: all|ip_prefix
    # multiple allows or denys is supported
    #deny: all
    #allow: 127.0.0.1
    #allow: 192.168
    # auth password must be at least 32 characters
    #auth: very-strong-password
    #readonly: yes
    # in ms, to log slowlog with WARN level
    #slowlog_timeout: 5

replication:
    binlog: yes
    # Limit sync speed to *MB/s, -1: no limit
    sync_speed: -1
    slaveof:
        # to identify a master even if it moved(ip, port changed)
        # if set to empty or not defined, ip:port will be used.
        #id: svc_2
        # sync|mirror, default is sync
        #type: sync
        #host: localhost
        #port: 8889

logger:
    level: debug
    output: log.txt
    rotate:
        size: 1000000000

leveldb:
    # in MB
    cache_size: 500
    # in MB
    write_buffer_size: 64
    # in MB/s
    compaction_speed: 1000
    # yes|no
    compression: yes
```

## [7.3 ssdb-admin](https://jshand.gitee.io/#/course/server/redis?id=_73-ssdb-admin)

```bash
docker run -d --env DB_CONFIG=172.18.0.4:8888 -p 5000:5000 --name ssdb-admin jhao104/ssdb-admin
```

通过上述命令后访问容器的5000端口（http://127.0.0.1:5000/ssdbadmin）可以查看到ssdbadmin的用户界面