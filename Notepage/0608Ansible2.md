# 0608

## hosts:不只可以放一台，可同時放多台(用,隔開)可放群組或是IP…

影片錄製補11:40~11:53(錄錯畫面…)…

var_public.yml→

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled.png)

## Project3(var_public.yml)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%201.png)

var_files:./vars_public.yml

(資料抓取放置位置)

#ansible-playbook playbook.yml

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%202.png)

## Project4

server1→servers

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%203.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%204.png)

遵循檔案名稱即可依序執行

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%205.png)

抓取server1,sever2裡面的app指令

→也就是ls group_vars裡面的檔案要如同你的hosts:一樣

#ansible-playbook playbook.yml

## Project5(host_var)

server1/servers→ ip

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%206.png)

## Project5(group_var++host_var

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%207.png)

## Project6(var_public.yml)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%208.png)

執行時才設定hosts為何

#ansible-playbook -e hosts=server1  playbook.yml

執行時才設定的參數優先權>yml內部撰寫

(盡量不要以免混亂)

## Project7

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%209.png)

記得去看戶號是否有被占用

有的話換一個

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2010.png)

shell:

register把上面的結果儲存:呼叫的名字

debug:

msg:"{{呼叫的名字}}”→簡單的打印方式

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2011.png)

重看看不懂看圖

應用：取的memory值

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2012.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2013.png)

cat /proc/meminfo | grep ……

取出容量的值

#ansible-playbook playbook.yml

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2014.png)

如果我想要取得我的CPU核心數要怎麼?

cat /proc/cpuinfo | grep processor

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2015.png)

cat /proc/cpuinfo | grep processor | wc -l

(計算行數)

## Project8

httpd.conf→複製一份→httpd.conf.j2

j2=jinja2

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2016.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2017.png)

hosts:webserver→servers

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2018.png)

ansible  servers -m setup

ansible  servers -m setup | grep -B 5 ipv4

補執行的命令

### 如果我想要取得我的CPU核心數要怎麼?

ansible  servers -m setup | grep cpu

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2019.png)

補執行的命令

## Project9

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2020.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2021.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2022.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2023.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2024.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2025.png)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2026.png)

待補

## Project10

可以使用萬用字去做更新

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2027.png)

避免萬一在執行上卡在第一步驟，可以使用ignore_errors: true

(圖片少了r)

![Untitled](0608%201486ec88f2f84124b98a28461827b882/Untitled%2028.png)