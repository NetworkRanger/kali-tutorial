利用Hashcat爆破linux密码学习记录
7月 ago
0x01

前言：

今天拿了个linux的主机，提下来了，以前提成root之后就没深入过，这次想着先把root密码破解出来；

以前交洞的时候只是单纯证明存在/etc/passwd和/etc/shadow，但从没管过里面的hash；

看网上教程也很多，我也记录一下我的学习成果吧。

0x02

大家都知道，linux系统中有一个用户密码配置文件 /etc/shadow ,里面存放着用户名以及一串密文：

形如：


root:$6$7vXyCOws$Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.:16821:0:99999:7:::
用户名:加密密码:上次更改密码的时间:最小更改密码间隔:密码有效期限:密码过期提示时间:密码锁定期:账户有效期:保留字段
1
2
root:$6$7vXyCOws$Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.:16821:0:99999:7:::
用户名:加密密码:上次更改密码的时间:最小更改密码间隔:密码有效期限:密码过期提示时间:密码锁定期:账户有效期:保留字段
另外一个 /etc/passwd 文件是用户账户配置文件,只保存用户账户的基本信息,并不保存密码信息。

形如：


root:x:0:0:root:/root:/bin/bash
用户名:密码:用户id:组ID:GECOS:主目录:默认Shell
1
2
root:x:0:0:root:/root:/bin/bash
用户名:密码:用户id:组ID:GECOS:主目录:默认Shell
0x03

由于咱们要破解是root密码，则只需要把/etc/shadow的root的加密密码拿出来即可；


$6$7vXyCOws$Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.
1
$6$7vXyCOws$Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.
最后小数点不要漏掉，因为这些文件内容格式都是:分割的，其余的都是内容；

这里来解释一下$分割的各个部分的含义：


6:表示一种类型标记为6的密码散列;
7vXyCOws:加盐(Salt)值；
Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.:hash值；
！具体也就是magic、salt、password
1
2
3
4
6:表示一种类型标记为6的密码散列;
7vXyCOws:加盐(Salt)值；
Hp/xoGf50Kov51cy83h6CTYoQerInkAFWWYZL22640N6P0kgy9Gfy4NVndDa1hNUevqR122E7ykmA1BIIOg0C.:hash值；
！具体也就是magic、salt、password
将这段加密密码保存到一个文件里，文件后缀.hash；

咱们这里保存为1.hash。

0x04

kali下的一款hash破解工具 hashcat ，网上说的天花乱坠，我这直接记录关于咱们破解root密码的具体用法，其他用法类似，具体百度吧；

hashcat据官网说牛逼得很，每秒最快可爆破80亿数据；

咱们这里利用他的暴力破解，就是常说的爆破，这也是得看字典；

具体命令：


hashcat -m 1800 -a 0 -o found.txt 1.hash 1.txt
1
hashcat -m 1800 -a 0 -o found.txt 1.hash 1.txt
解释一下：


-m 是指定那种加密类型，1800是SHA-512(Unix)的代号，具体--help来查看；
-a  是指定攻击模式,0代表Straight模式,使用字典进行破解尝试;
-o 是破解出来的信息输出结果文件,输出到found.txt文件中;
1.hash 是我们上面保存的加密密码文件；
1.txt 是我们的爆破密码，越大越精越好；
1
2
3
4
5
-m 是指定那种加密类型，1800是SHA-512(Unix)的代号，具体--help来查看；
-a  是指定攻击模式,0代表Straight模式,使用字典进行破解尝试;
-o 是破解出来的信息输出结果文件,输出到found.txt文件中;
1.hash 是我们上面保存的加密密码文件；
1.txt 是我们的爆破密码，越大越精越好；
这里碰到了一点小问题，hash-identifier 来看root密码检查是 SHA-256 加密，但这个并不是我这台机子linux的加密方式，使用这个是不能开始爆破的；

20160820221925

但这可以先记住一点：


linux 的/etc/shadow文件中hash算法包括缺省的DES经典算法、MD5哈希算法($1)、Blowfish加密算法($2或$2a)和SHA哈希算法($5或$6）。
因此利用hashcat进行破解的参数也不同，比如MD5哈希算法($1)，使用hashcat -m 500参数；
SHA哈希算法($5或$6），使用hashcat -m 1800 参数。
1
2
3
linux 的/etc/shadow文件中hash算法包括缺省的DES经典算法、MD5哈希算法($1)、Blowfish加密算法($2或$2a)和SHA哈希算法($5或$6）。
因此利用hashcat进行破解的参数也不同，比如MD5哈希算法($1)，使用hashcat -m 500参数；
SHA哈希算法($5或$6），使用hashcat -m 1800 参数。
具体标记如下：


1. $0 = DES
2. $1 = MD5
3. $2a(2y) = Blowfish
4. $5 = SHA-256
5. $6 = SHA-512
1
2
3
4
5
1. $0 = DES
2. $1 = MD5
3. $2a(2y) = Blowfish
4. $5 = SHA-256
5. $6 = SHA-512
这里可以直接看magic标记值来直接判断，这里是6，所以是SHA-512的magic值1800；

回车开始爆破。

0x05

拿本地操作一下，放了个6000小字典，里面放上了虚拟机root的密码；

20160820221718

可以看到，很快就爆破出来了，每秒近700次，还是挺快的；

结果会在当前目录生产两个文件found.txt和hashcat.pot；

里面内容一样，都是爆破出来的结果；

20160820221758

0x06

1.hashcat支持市面上存在的近乎全部的hash加密，–help可以看到；

2.在并不知道密文的情况下，我们可以先使用hash-identifier来帮助检测下，虽然有的不准确；

3.各种加盐（salt）的顺序，以及hash值的具体得来的方式不同，也会导致加密的类型产生差别；

如同：


0 = MD5
10 = md5($pass.$salt)
20 = md5($salt.$pass)
300= md5(unicode($pass).$salt)
40 = md5($salt.unicode($pass))
3300 = MD5(Sun)
3500 = md5(md5(md5($pass)))
3610 = md5(md5($salt).$pass)
3710 = md5($salt.md5($pass))
3720 = md5($pass.md5($salt))
3800 = md5($salt.$pass.$salt)
3910 = md5(md5($pass).md5($salt))
4010 = md5($salt.md5($salt.$pass))
4110 = md5($salt.md5($pass.$salt))
4210 = md5($username.0.$pass)
4300 = md5(strtoupper(md5($pass)))
4400 = md5(sha1($pass))
....
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
0 = MD5
10 = md5($pass.$salt)
20 = md5($salt.$pass)
300= md5(unicode($pass).$salt)
40 = md5($salt.unicode($pass))
3300 = MD5(Sun)
3500 = md5(md5(md5($pass)))
3610 = md5(md5($salt).$pass)
3710 = md5($salt.md5($pass))
3720 = md5($pass.md5($salt))
3800 = md5($salt.$pass.$salt)
3910 = md5(md5($pass).md5($salt))
4010 = md5($salt.md5($salt.$pass))
4110 = md5($salt.md5($pass.$salt))
4210 = md5($username.0.$pass)
4300 = md5(strtoupper(md5($pass)))
4400 = md5(sha1($pass))
....
4.这里linux的密码只看$分割，其余字符都是内容，有点 . 也得带上。