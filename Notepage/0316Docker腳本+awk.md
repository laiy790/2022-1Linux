# 0316Docker腳本+awk

# gedit prepare_web.sh

```c
!/usr/bin/bash
for i in {1..5};
do
  mkdir -p myweb$i
  cd myweb$i
  echo $i > hi.htm
  cd ..
done
```

# chmod +x [prepare_web.sh](http://prepare.sh/)
# ./prepare_web.sh

# gedit docker_httpd_setup5.sh

```c
!/usr/bin/bash

for i in {1..5};
do

  portno=`expr 9000 + $i`
  docker run -d -p $portno:80 -v /home/user/myweb$i:/usr/local/apache2/htdocs httpd
done
```

 # chmod +x docker_httpd_setup5.sh
 # ./docker_httpd_setup5.sh

gedit Dockerfile

 # cat Dockerfile
FROM centos:centos7
RUN yum -y install httpd
EXPOSE 80
ADD index.html /var/www/html

 # echo "hello world" > index.html
 # docker build -t myhttpd:1.0 .

 # docker run -d -p 8888:80 myhttpd:1.0 /usr/sbin/apachectl -DFOREGROUND
16bbe18dad8a65fca83f835904c9d9a01489ceedce9e420b784cb7afa65cbbde

 touch haproxy.cfg
defaults
mode http
timeout client 10s
timeout connect 5s
timeout server 10s
timeout http-request 10s

frontend myfrontend
  bind 0.0.0.0:8080
  default_backend myservers

backend myservers
  balance roundrobin
  server server1 192.168.23.102:9001
  server server2 192.168.23.102:9002
  server server3 192.168.23.102:9003
  server server4 192.168.23.102:9004
  server server5 192.168.23.102:9005

 # docker run -p 8080:8080 -d --name haproxy-master -v /home/user/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg --privileged=true haproxy
acdc64e7e0f08bd2c9650f225c3163f17382889550bb975145a5ba3546028038
 # docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
acdc64e7e0f0   haproxy   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   haproxy-master

 # docker logs acd
[NOTICE]   (1) : New worker (8) forked
[NOTICE]   (1) : Loading success.

### 

 # docker pull faucet/python3:0.0.1
Using default tag: latest
latest: Pulling from faucet/python3:0.0.1
59bf1c3509f3: Pull complete
309cc9d30407: Pull complete
0b8d4d1796c6: Pull complete
9de4cabd6442: Pull complete
Digest: sha256:2deeb32c07d6eee2ae0d57a643f2ec11f392804076920af02c17163c945728ce
Status: Downloaded newer image for faucet/python3:latest
[docker.io/faucet/python3:latest](http://docker.io/faucet/python3:latest)
 # docker run -it -v /home/user:/mydata faucet/python3:0.0.1 bash
Starting with UID=0 GID=0
bash-5.1# cd mydata/
bash-5.1# python3 [test.py](http://test.py/)
hello world

awk 指令用法

 ifconfig enp0s8 | grep netmask
inet 192.168.56.104  netmask 255.255.255.0  broadcast 192.168.56.255

 ifconfig enp0s8 | grep netmask | awk '{print $2}'
192.168.56.104

 ifconfig enp0s8 | awk '/netmask/ {print $2}'
192.168.56.104

 ifconfig enp0s8 | awk 'NR==2 {print $2}'
192.168.56.104

 ifconfig enp0s8 | head -n 2 | tail -n 1
inet 192.168.56.104  netmask 255.255.255.0  broadcast 192.168.56.255

 cat file
11 dywang 81 12 A
152 linda 90 58 C
33 peter 72 95 C
4 rita 65 34 E
58 cora 5 85 D

 awk 'BEGIN{sum=0} {sum+=$3;} END {printf("SUM:%d\n",sum)}' file
SUM:313

### gedit process.awk

 # cat process.awk
BEGIN{
sum=0;
i=0;
}
{
sum+=$3;
i++;
}
END{
printf("SUM = %d\n",sum);
printf("AVG = %.2f\n",sum/i);
}
 # awk -f process.awk file
SUM = 313
AVG = 62.60

### gedit process_max.awk

# cat process_max.awk
BEGIN{
sum=0;
max_name="";
}
{
tmp_sum=$3+$4;
#printf("tmp_sum=%d sum=%d\n",tmp_sum,sum);
if(tmp_sum > sum){
sum=tmp_sum;
max_name=$2;
}
}
END{
printf("MAX = %s\n",max_name);
printf("SCORE = %d\n",sum);
}
 # gawk -f process_max.awk file
MAX = peter
SCORE = 167

-F 刪除

 # cat file
11,dywang,81,12,A
152,linda,90,58,C
33,peter,72,95,C
4,rita,65,34,E
58,cora,5,85,D

 # awk -F, '{print $2}' file
dywang
linda
peter
rita
cora