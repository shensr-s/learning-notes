## 修改密码

更改mysql（5.7版本以上）密码命令：
set password for 'root'@'localhost'=password('新的密码');
如果报 “The MySQL server is running with the --skip-grant-tables option so it cannot exe”错误，先 flush privileges;然后在进行更改