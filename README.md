# CTFDefense

Some tools for CTF off line

![](imgs/20171001-103837.png)

### 1.ctf-firewall.sh
    iptables配置命令，可一键执行快速配置。使用时可能需要根据环境略微修改


### 3.getRoot
	一些本地提权poc，每个文件里都有详细的使用注释，编译好的文件在release目录下。还有几个实用脚本

![](imgs/20171001-104020.png)

### 4.Monitor
	一个简单的文件监控示例脚本，可以监控创建、删除、移动、属性修改操作，自动删除新增文件或目录。已使用pyinstaller打包成了linux可执行程序
![](imgs/20171001-105054.png)

### 5. WAF
	linux版安全狗和几个waf脚本

![](imgs/20171001-105010.png)



### AWD注意事项：

### ssh操作
	ssh <-p 端口> 用户名@IP　　//登录
	scp 文件路径  用户名@IP:存放路径　　//向ssh服务器上传输文件
	
### 备份web目录
	tar -zcvf web.tar.gz /var/www/html/

### 查看已建立的网络连接以及对应进程
	netstat -antulp | grep EST

### 用户管理
	w 　　//查看当前用户
	pkill -kill -t <用户tty>　　 //踢掉当前登录用户

### 进程管理
	查看进程信息
	ps aux | grep pid或者进程名　　
	
	查看指定端口被哪个进程占用
	lsof -i:端口号 
	或者 netstat -tunlp|grep 端口号
	
	结束进程命令
	kill PID
	killall <进程名>
	kill -9 <PID>
	
### iptables命令
	封杀某个IP或者ip段，如：123.4.5.6
	iptables -I INPUT -s 123.4.5.6 -j DROP
	iptables -I INPUT -s 123.4.5.1/24 -j DROP
	
	禁止从某个主机ssh远程访问登陆到本机，如123.4.5.6
	iptable -t filter -A INPUT -s 123.4.5.6 -p tcp --dport 22 -j DROP

## Mysql数据库操作
	
	备份mysql数据库
	mysqldump -u 用户名 -p 密码 数据库名 > back.sql　　//备份指定数据库
	mysqldump --all-databases > bak.sql　　　　//备份所有数据库
	
	还原mysql数据库
	mysql -u 用户名 -p 密码 数据库名 < bak.sql

### 安全检查
	
	find / *.php -perm 4777 　　 //查找777的权限的php文件 
	awk -F: '{if($3==0)print $1}' /etc/passwd　　//查看root权限的账号
	crontab -l　　//查看计划任务
	
	检测所有的tcp连接数量及状态
	netstat -ant|awk '{print $5 "\t" $6}' |grep "[1-9][0-9]*\."|sed -e 's/::ffff://' -e 's/:[0-9]*//'|sort|uniq -c|sort -rn
	　　
	查看页面访问排名前十的IP
	cat /var/log/apache2/access.log  | cut -f1 -d " " | sort | uniq -c | sort -k 1 -r | head -10
	　　
	查看页面访问排名前十的URL
	cat /var/log/apache2/access.log  | cut -f4 -d " " | sort | uniq -c | sort -k 1 -r | head -10　　
