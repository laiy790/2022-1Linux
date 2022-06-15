# 0323rsync

[https://blog.tomy168.com/2019/01/centos-76x64-rsync-daemon.html](https://blog.tomy168.com/2019/01/centos-76x64-rsync-daemon.html)

[Linux 使用 rsync 遠端檔案同步與備份工具教學與範例 - G. T. Wang (gtwang.org)](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)

**server**

yum install rsync

cp /etc/rsyncd.conf /etc/rsyncd.conf-bak

gedit rsyncd.conf

```c
# /etc/rsyncd: configuration file for rsync daemon mode

# See rsyncd.conf man page for more options.

# configuration example:

# uid = nobody
# gid = nobody
# use chroot = yes
# max connections = 4
# pid file = /var/run/rsyncd.pid
# exclude = lost+found/
# transfer logging = yes
# timeout = 900
# ignore nonreadable = yes
# dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2

# [ftp]
#        path = /home/ftp
#        comment = ftp export area

uid=root
gid=root
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
secrets file = /etc/rsyncd.passwd

[mod1]
    path = /backup1
    read only = no
    auth users = vuser
```

gedit rsyncd.passwd

```c
vuser:123456
```

chmod 600 /etc/rsyncd.paswd

ntpdate time.google.com (同步時間)

systemctl restart rsyncd

**client**

gedit /root/rsync_user.passwd

```c
123456
```

chmod 600 /root/rsync_user.passwd

rsync -avz --progress --password-file=/etc/rsync_vuser.passwd testdir/ vuser@192.168.23.@@@::mod1

---

同一主機2

yum install rsync
rpm -qa | grep rsync 
echo "hi" > a.txt
rsync -avh a.txt /tmp

cd /tmp
ls
cat a.txt

mkdir testdir  
touch {1..10}.txt
ls
cd ..
rsync -avh testdit/ /tmp

cd testdir/
echo "1" >1.txt
cd ..
rsync -avh testdir/ /tmp

原理是:mdsum5 (雜湊hash) 修改的話 再一次md5sum就會不一樣
cd testdir/
rsync 比較兩邊md5sum 把不同做備份
touch 11.txt 12.txt
cd ...
rsync -avh testdir/ /tmp

刪除:
cd testdir/
rm 4.txt 5.txt
rsync -avh testdir/ /tmp
不會動 預設是不改動
要加參數告訴她

rsync -avh --delete testdir/ /tmp

part2 備份
cd testdir
mkdir a/b/c -p  /// a>b>c  ///-p 如果沒a就幫建 沒b就幫 沒c就幫
tree

開新vb
cd ..
cd tmp
mkdir backup
會到7-5(應該第一台)
rsync -avzh testdit/ user@192.168.56.@@@:/tmp/backup/

要做ssh認證 所以要先做免密登入
ssh-keygen
ssh-copy-id user@192.168.56.@@@
ssh user@192.168.56.@@@
rsync -avzh testdit/user@192.168.56.@@@:/tmp/backup/

如果遠端富號有變 就改如下
rsync -avzh -e "ssh" -p 22 testdir/ user@192.168.56.@@@:/tmp/backup/

cd testdir/
touch {a..c}.txt
tree
rsync -avzh --exclude 'a.txt' --exclude 'b.txt' testdit/ [user@192.168.56.XXX](mailto:user@192.168.56.XXX):/tmp/backup/

也可以設定最大最小的備份檔案大小值

剛剛都是remote shell 現在要跑在伺服器 也可創vuser 跟系統使用者分開

產生配置黨 放在etc下叫rsync.conf /////放在伺服器端
cd /etc
ls rsync.conf
gedit rsync.conf
uid=root   #使用者身分別
gid=root   #群組
身分別
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
secrets file = /etc/rsyncd.passwd     # 虛擬使用者 密碼放這裡

###############mod-相關參數與配置 整合

[mod1]
path = /backup1
read only = no  
auth users = vuser  

虛擬器(7-6
gedit /etc/rsync.passwd
vuser:user(密碼)
chmod 600 /etc/rsyncd.passwd

遠端與本地時間要同步

ntpdate watch.stdtime.gov.tw
兩台都做
date確認
7-6 (伺服器)
systemctl start rsyncd
cat /var/run/rsyncd.pid
cat /var/log/rsyncd.log

mkdir /backup1

7-5 (客戶端)
gedit /etc/rsync_vuser.passwd

放密碼
user
chomd 600 /etc/rsync_vuser_passwd
ls
rsync -avz --progress --password-file=/etc/rsync_vuser.passwd testdir/ vuser@192.168.56.@@@::mod1

[總Page](https://github.com/laiy790/2022Linux/blob/main/Note.md)