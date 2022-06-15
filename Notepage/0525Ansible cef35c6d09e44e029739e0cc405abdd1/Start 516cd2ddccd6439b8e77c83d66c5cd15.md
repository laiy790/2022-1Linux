# Start

綠色代表執行成功

ansible servers -m ping

確認開機時間

# ansible server1 -m command -a "uptime"

# ansible -m command -a "uptime" 'server1'

查看記憶體

# ansible  -m command -a "free -m" server1

新增使用者

# ansible server1 -m command -a "useradd amy"

# ansible server1 -m command -a "id amy"(確認存在與否)

---