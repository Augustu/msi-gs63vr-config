### Redis

ref: https://blog.csdn.net/miss1181248983/article/details/90056960

#### Install

```bash
# for centos 7, install gcc first, ref: centos7.md
yum install -y systemd-devel.x86_64 tcl.x86_64

make USE_SYSTEMD=yes MALLOC=libc install && make test
# debian only
# cd utils/ && ./install_server.sh

cp redis.conf /etc/

useradd redis -s /sbin/nologin -d /var/lib/redis

vi /usr/lib/systemd/system/redis.service
[Unit]
Description=Redis persistent key-value database
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/redis-server /etc/redis.conf --supervised systemd
ExecStop=/usr/libexec/redis-shutdown
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target


vi /usr/libexec/redis-shutdown
#!/bin/bash
#
# Wrapper to close properly redis and sentinel
test x"$REDIS_DEBUG" != x && set -x

REDIS_CLI=/usr/local/bin/redis-cli

# Retrieve service name
SERVICE_NAME="$1"
if [ -z "$SERVICE_NAME" ]; then
   SERVICE_NAME=redis
fi

# Get the proper config file based on service name
CONFIG_FILE="/etc/$SERVICE_NAME.conf"

# Use awk to retrieve host, port from config file
HOST=`awk '/^[[:blank:]]*bind/ { print $2 }' $CONFIG_FILE | tail -n1`
PORT=`awk '/^[[:blank:]]*port/ { print $2 }' $CONFIG_FILE | tail -n1`
PASS=`awk '/^[[:blank:]]*requirepass/ { print $2 }' $CONFIG_FILE | tail -n1`
SOCK=`awk '/^[[:blank:]]*unixsocket\s/ { print $2 }' $CONFIG_FILE | tail -n1`

# Just in case, use default host, port
HOST=${HOST:-127.0.0.1}
if [ "$SERVICE_NAME" = redis ]; then
    PORT=${PORT:-6379}
else
    PORT=${PORT:-26739}
fi

# Setup additional parameters
# e.g password-protected redis instances
[ -z "$PASS"  ] || ADDITIONAL_PARAMS="-a $PASS"

# shutdown the service properly
if [ -e "$SOCK" ] ; then
        $REDIS_CLI -s $SOCK $ADDITIONAL_PARAMS shutdown
else
        $REDIS_CLI -h $HOST -p $PORT $ADDITIONAL_PARAMS shutdown
fi


chmod +x /usr/libexec/redis-shutdown

systemctl daemon-reload
systemctl start redis



```



### Master-slaver

```bash
# c1




```









