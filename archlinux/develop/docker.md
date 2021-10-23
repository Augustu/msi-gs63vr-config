### docker

#### Install

```bash
sudo vim /etc/docker/daemon.json
{
	"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}

sudo systemctl restart docker

sudo usermod -a -G users $USER
```



#### proxy

```bash
# proxy

sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:1080"
Environment="HTTPS_PROXY=http://127.0.0.1:1080"
Environment="NO_PROXY=localhost,127.0.0.1,docker.mirrors.ustc.edu.cn"
```

#### mysql

```bash
docker run -p 127.0.0.1:3306:3306 --name mysql56 \
	-v /home/augustu/Mount/Develop/data/mysql56/conf:/etc/mysql \
	-v /home/augustu/Mount/Develop/data/mysql56/mysql:/var/lib/mysql \
	-v /home/augustu/Mount/Develop/data/mysql56/mysql-files:/var/lib/mysql-files \
	-v /home/augustu/Mount/Develop/data/mysql56/logs:/var/log/mysql \
	-e MYSQL_ROOT_PASSWORD=123456 \
	-d mysql:5.6


```

#### registry

```bash
# store private docker image

docker run -itd --name registry --restart=always  -p 35000:5000 -v /home/mike/registry:/var/lib/registry registry
```

