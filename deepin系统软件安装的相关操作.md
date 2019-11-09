# deepin系统软件安装的相关操作

 tags:deepin mysql安装 jdk安装

---

## mysql的安装与配置
1.mysql卸载
    mysql -V（查版本）
    
    apt-get autoremove --purge mysql-server-5.5.36(此处为版本号)
    
    apt-get autoremove mysql-server
    
    apt-get remove mysql-common
    
    dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
    
    如果报如下错误，证明你的系统中没有残留配置文件了，无须担心。
    
    dpkg: error: --purge 需要至少一个软件包名作为参数
    
    输入 dpkg --help 可获得安装和卸载软件包的有关帮助 [*]；
    
    使用 dselect 或是 aptitude 就能在友好的界面下管理软件包；
    
    输入 dpkg -Dhelp 可看到 dpkg 除错标志的值的列表；
    
    输入 dpkg --force-help 可获得所有强制操作选项的列表；
    
    输入 dpkg-deb --help 可获得有关操作 *.deb 文件的帮助；
    
    带有 [*] 的选项将会输出较大篇幅的文字 - 可使用管道将其输出连接到 less 或 more ！
    
2.mysql的安装与配置

	安装：
	
	sudo apt-get install mysql-server mysql-client
	
	配置：
	
	(1)cat /etc/mysql/debian.cnf
	
	记录下user和password的内容
	
	(2)mysql -u user -p
	
	(3)输入密码，回车(密码为空时，可在上一行命令前加sudo，再回车就进入到数据库中)
	
	(4)mysql> use mysql;
	
	(5)mysql> select host,user,plugin,authentication_string from user; 
	
	(6)mysql> update user set plugin="mysql_native_password",authentication_string=password('新密码') where user="root"; 
	
	(7)mysql> FLUSH PRIVILEGES;
	
	(8)退出数据库后重新登陆。
	

## JDK环境变量的配置

1.jdk下载

    在oracle官网(https://www.oracle.com/technetwork/java/javase/downloads/index.html)下载压缩包，并解压，记录下jdk的安装路径。
    
2.配置

 	（1）sudo vim /etc/profile
	
 	（2）键入G定位到文末，键入i在当前位置插入，进行编辑
	
      	（3）JAVA_HOME=/usr/lib/jvm/jdk-13.0.1
	  PATH=\$JAVA_HOME/bin:$PATH
	  export JAVA_HOME  PATH 
		
	  其中JAVA_HOME的值是jdk的安装路径。
		
	（4）source /etc/profile (配置立即生效)
	
	（5）重启后，可永久生效。




