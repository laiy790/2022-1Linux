# 0601-ansible

[太厲害了，終於有人能把Ansible講的明明白白了，建議收藏 - tw511教學網](https://tw511.com/a/01/32123.html)

user模組&****playbook(太麻煩晚點用)****

老師丟群組的檔案

下載後利用Winscp傳至linux7-1

的home/user/ansible/project1裡面

gedit home/user/ansible/project1/httpd.conf(不太確定是叫這個嗎ls一下看看唄)

gedit home/user/ansible/project1/playbook.yml

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled.png)

yml 注重縮排跟空格

7-2 

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%201.png)

rpm -qa | grep http

rpm -e httpd

rpm -qa | grep http

netstat -tunlp | grep 80

7-1

ansible-playbook

ansible-playbook playbook.yml

綠色回應=沒改變

黃色回應=改變

curl http://centos7-2

- [ ]  截圖

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%202.png)

示範((gedit home/user/ansible/project1/playbook.yml

第一行改成myserver的話是執行自己的機器(7-2,7-3)

))

示範

gedit home/user/ansible/project1/playbook.yml

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%203.png)

gedit home/user/ansible/project1/httpd.conf

找到80號改成8080

httpd.conf可以自行用下載的方式建置

示範

gedit home/user/ansible/project1/playbook.yml

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%203.png)

圖片最後一行改state=restarted

盡量在活躍網站不要使用restarted會把在線用戶登出

gedit home/user/ansible/project1/httpd.conf

找到80號改成8081

.

.

.

gedit home/user/ansible/project1/playbook.yml

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%204.png)

```jsx
- hosts: server1
  tasks:
    - name: install httpd server
      yum: name=httpd state=present

    - name: configure httpd server
      copy: src=./httpd.conf dest=/etc/httpd/conf/httpd.conf
      notify: reload httpd server

    - name: start httpd server
      service: name=httpd state=started enabled=yes

  handlers:
    - name: reload httpd server
      service: name=httpd state=reloaded
```

notify觸發handlers(名字一定要一樣)

gedit home/user/ansible/project1/httpd.conf

改成8085

curl http://地2台ip:8080

### PLAYBOOK練習

[Linux - FTP vsftpd 安裝及設定教學 (hoohoo.top)](https://hoohoo.top/blog/linux-ftp-teaching/)

```jsx
- hosts: server1
  tasks:
    - name: install vsftpd server
      yum: name=vsftpd state=present

    #- name: configure vsftpd server
     # copy: src=./vsftpd.conf dest=/etc/vsftpd/vsftpd.conf
     # notify: reload vsftpd server

    - name: start vsftpd server
      service: name=vsftpd state=started enabled=yes

 # handlers:
   # - name: reload vsftpd server
   #   service: name=vsftpd state=reloaded
```

ansible-playbook pkaybook.yml

yum install vsftpd

ftp centos7-2登陸

- [ ]  截圖

成功後

```jsx
- hosts: server1
  tasks:
    - name: install vsftpd server
      yum: name=vsftpd state=present

    - name: configure vsftpd server
      copy: src=./vsftpd.conf dest=/etc/vsftpd/vsftpd.conf
      notify: restarted vsftpd server

    - name: start vsftpd server
      service: name=vsftpd state=started enabled=yes

  handlers:
    #- name: stopped vsftpd server
    #  service: name=vsftpd state=stopped

		#- name: started vsftpd server
    #  service: name=vsftpd state=started

		- name: restart vsftpd server
      service: name=vsftpd state=restarted
```

把vsftpd.conf內容改掉(原配置檔)改成下方的

anonymous_enable=YES改成NO的話anonymous不允許登陸

```jsx
# Example config file /etc/vsftpd/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
anonymous_enable=YES
#
# Uncomment this to allow local users to log in.
# When SELinux is enforcing check for SE bool ftp_home_dir
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
# When SELinux is enforcing check for SE bool allow_ftpd_anon_write, allow_ftpd_full_access
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/xferlog
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode. The vsftpd.conf(5) man page explains
# the behaviour when these options are disabled.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd/banned_emails
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
#chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd/chroot_list
#

smallko, [2022/6/1 上午 11:16]
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# When "listen" directive is enabled, vsftpd runs in standalone mode and
# listens on IPv4 sockets. This directive cannot be used in conjunction
# with the listen_ipv6 directive.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
# Make sure, that one of the listen options is commented !!
listen_ipv6=YES

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YESS

```

如果以上需要簡單總結的話

cat vsftpd.conf | grep -v ^# | grep -v ^$

![Untitled](0601-ansible%200b5efd1e2b5c4acba9e311a2e14541a2/Untitled%205.png)

ansible-playbook playbook.yml

新增帳號

```jsx
- hosts: server1
  tasks:
    - name: install vsftpd server
      yum: name=vsftpd state=present

    - name: configure vsftpd server
      copy: src=./vsftpd.conf dest=/etc/vsftpd/vsftpd.conf
      notify: restarted vsftpd server

    - name: start vsftpd server
      service: name=vsftpd state=started enabled=yes

		- name: add a new user "alben"
			user:
				name:"alben"
				password: "{{'albenpw'|password_hash('sha512')}}"

  handlers:
    #- name: stopped vsftpd server
    #  service: name=vsftpd state=stopped

		#- name: started vsftpd server
    #  service: name=vsftpd state=started

		- name: restart vsftpd server
      service: name=vsftpd state=restarted
```

💫ansible server1 -m user -a “name=tom password={{’tom’|password_hash(’sha512’)}}

設定密碼不能用明文方式設定，必須以{{'使用者密碼'|password_hash('雜湊值類型?')}}

```jsx
- hosts: server1
	vars:
		- app1: httpd
		- app2: vsftpd
  tasks:
    - name: install {{ app1 }} and {{ app2 }}
      yum:
				name:
					- "{{ app1 }}"
					- "{{ app2 }}"
				state: present

  
```