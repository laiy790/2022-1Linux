# 0330~0413JumpServer

抓不到ip

sudo curl -L "[https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$](https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$)(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

[root@weinote user]# systemctl start docker
[root@weinote user]# systemctl status docker

[https://github.com/jumpserver/Dockerfile](https://github.com/jumpserver/Dockerfile)

```
git clone --depth=1 https://github.com/wojiushixiaobai/Dockerfile.git
cd Dockerfile
cp config_example.conf .env
docker-compose -f docker-compose-redis.yml -f docker-compose-mariadb.yml -f docker-compose.yml up
```

[root@weinote user]# gedit .env

![1648606955662.jpg](0330~0413JumpServer%20476adefdb5ca4dddb6338c9a594153a4/1648606955662.jpg)

有錯誤的話砍掉

```c
[root@weinote user]# docker-compose down
[root@weinote user]# docker ps -a
[root@weinote user]# docker rm -f (帶有jms 的檔案）
[root@weinote user]# docker-compose -f docker-compose-redis.yml -f docker-compose-mariadb.yml -f docker-compose.yml up
```

……

---

+gmail

[總Page](https://github.com/laiy790/2022Linux/blob/main/Note.md)