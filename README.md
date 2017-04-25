#### CentOS 6.5服务器配置日志

[TOC]

##### 安装nginx

> 1. 服务器相关
>
> ```
> $ servernamne :  ssh root@120.25.198.16
> $ password: QWer123.0
> ```
>
> 1. 通过yum安装
>
> ```shell
> #添加nginx的yum源
> $ rpm -ivh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm 
> $ yum install nginx 
> $ yum info nginx
> $ yum 
> ```
>
> 1. 配置nginx，支持php解析
>
> ```shell
> server {
> listen       80;
> server_name  localhost;
>
> #charset koi8-r;
> #access_log  /var/log/nginx/log/host.access.log  main;
>
> location / {
> root   /usr/share/nginx/html;
> index  index.php index.html index.htm;
> }
>
> #error_page  404              /404.html;
>
> # redirect server error pages to the static page /50x.html
> #
> error_page   500 502 503 504  /50x.html;
> location = /50x.html {
> root   /usr/share/nginx/html;
> }
>
> # proxy the PHP scripts to Apache listening on 127.0.0.1:80
> # 
> #location ~ \.php$ {
> #    proxy_pass   http://127.0.0.1;
> #}
>
> # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
> #
> location ~ \.php$ {
> root           /usr/share/nginx/html;
> fastcgi_pass   127.0.0.1:9000;
> fastcgi_index  index.php;
> fastcgi_param  SCRIPT_FILENAME  /usr/share/nginx/html$fastcgi_script_name;
> include        fastcgi_params;
> }
>
> # deny access to .htaccess files, if Apache's document root
> # concurs with nginx's one
> #
> #location ~ /\.ht {
> #    deny  all;
> #}
> }
> ```
>
> 1. over

##### 安装mysql

> 1. 安装mysql
>
> ```shell
> #安装mysql
> $ yum -y install mysql-server 
> #按轧辊php-mysql
> $ yum -y install php-mysql 
> ```
>
> 1. 编辑mysql配置文件
>
> ```shell
> [root@sample ~]#vim /etc/my.cnf　 ← 编辑MySQL的配置文件
> [mysqld]
> datadir=/var/lib/mysql
> socket=/var/lib/mysql/mysql.sock
> # Default to using old password format for compatibility with mysql 3.x
> # clients (those using the mysqlclient10 compatibility package).
> old_passwords=1　 ← 找到这一行，在这一行的下面添加新的规则，让MySQL的默认编码为UTF-8
> default-character-set = utf8　 ← 添加这一行
> 然后在配置文件的文尾填加如下语句：
> [mysql]
> default-character-set = utf8
> ```
>
> 1. mysql启动报错
>
> ```
> ERROR 2002(HY000):Can't connect to local MySQL server through socket 'var/lib/mysql/.mysql.sock'(2)
> #先查看 
> $ /etc/rc.d/init.d/mysqld status 看看mysql是否启动
> ＃确定你的mysql.sock 是不是在那个位置
> $ mysql -u mysqlusername -p -S /var/lib/mysql/mysql.sock
> #试试service mysqld start 
> #如果是权限问题， 先改变权限
> $ chown -R mysql:mysql /var/lib/mysql
> #启动mysql
> $ mysql -uroot -p 
> ```
>
> 1. over.

##### 常用命令

> ```shell
> $ service nginx start  #启动nginx
> $ service nginx stop   #停止nginx
> $ service nginx reload  #重启nginx
> $ /usr/local/php/sbin/php-fpm  #启动php
> $ killall -9 php-fpm  #停止php
> $ service mysqld start  #启动mysqd
> $ mysql -uroot -p #启动mysql
> ```

##### 常用目录

> ```shell
> * nginx 配置文件目录：/etc/nginx/*.conf
> * php配置文件目录：/usr/local/php/etc/php.ini
> * 网页文件目录：/usr/share/nginx/html (将.php \ .html文件放置在此，外部即可访问。)
> ```

##### Linux常用命令

> ```shell
> #curl是Linux/Unix下的一个开源文件请求命令。
> $ curl [option] [URL]z
> $ curl Url... 返回指定url中的数据并打印在屏幕上
> ```





##### 安装前准备

> 1. 服务器密码和用户名
>
> ```
> $ ssh root@120.25.198.16
> $ password: QWer123.0
> ```
>
> 1. 查看系统版本
>
> ```shell
> lsb_release -a  
> #-a , --all
> LSB是Linux Standard Base的缩写，lsb_release命令用来显示LSB和特定的版本相关信息。
> man lsb_release 
> lsb_relese -v / -i / -r / -d / -c / -h
> ```
>
> 1. 首先系统安装完后，执行：`yum -y update`命令，升级补丁。该命令是升设置和系统设置，系统版本和内核都会升级。
> 2. Linux常用目录结构
>
> ```shell
> [root@iZ942i1e5bzZ ~]# ls -l /
> 总用量 96
> dr-xr-xr-x.  2 root root  4096 3月  26 20:02 bin
> dr-xr-xr-x.  4 root root  4096 3月  26 20:02 boot
> drwxr-xr-x  16 root root  3360 3月  26 20:02 dev
> drwxr-xr-x. 81 root root  4096 3月  26 20:02 etc
> drwxr-xr-x.  2 root root  4096 9月  23 2011 home
> dr-xr-xr-x. 11 root root  4096 3月  26 20:00 lib
> dr-xr-xr-x.  9 root root 12288 3月  26 20:02 lib64
> drwx------.  2 root root 16384 8月  14 2014 lost+found
> drwxr-xr-x.  2 root root  4096 9月  23 2011 media
> drwxr-xr-x.  2 root root  4096 9月  23 2011 mnt
> drwxr-xr-x.  3 root root  4096 8月  14 2014 opt
> dr-xr-xr-x  77 root root     0 3月  24 22:17 proc
> dr-xr-x---.  2 root root  4096 2月  22 14:18 root
> dr-xr-xr-x.  2 root root 12288 3月  26 20:02 sbin
> drwxr-xr-x.  2 root root  4096 8月  14 2014 selinux
> drwxr-xr-x.  2 root root  4096 9月  23 2011 srv
> drwxr-xr-x  13 root root     0 3月  24 22:17 sys
> drwxrwxrwt.  3 root root  4096 3月  26 20:18 tmp
> drwxr-xr-x. 13 root root  4096 8月  14 2014 usr
> drwxr-xr-x. 20 root root  4096 8月  14 2014 var 
> ```
>
> 1. FHS原则
>
> |                | 可分享的(shareable)                          | 不可分享的(unshakable)               |
> | -------------- | ---------------------------------------- | ------------------------------- |
> | 不变的(static)    | /usr(软件放置处)  /opt(第三方协议软件)               | /etc(配置文件)  /boot(开机与核心档)       |
> | 可变动的(variable) | /var/mail (使用者邮件信箱) /var/spool/news(新闻组) | /var/run(程序相关) /var/lock( 程序相关) |
>
> 1. FHS认为跟目录(／)下应该有如下子目录：
>
> 2. | 目录    | 应放置档案内容                                  |
>    | ----- | :--------------------------------------- |
>    | /bin  | 系统有很多放置执行档的目录，但/bin比较特殊。因为/bin放置的是在单人维护模式下还能够被操作的指令。在/bin底下的指令可以被root与一般帐号所使用，主要有：cat,chmod(修改权限), chown, date, mv, mkdir, cp, bash等等常用的指令。 |
>    | /boot | 主要放置开机会使用到的档案，包括Linux核心档案以及开机选单与开机所需设定档等等。Linux kernel常用的档名为：vmlinuz ，如果使用的是grub这个开机管理程式，则还会存在/boot/grub/这个目录。 |
>    | /dev  | 在Linux系统上，任何装置与周边设备都是以档案的形态存在于这个目录当中。 只要通过存取这个目录下的某个档案，就等于存取某个装置。重要的档案有/dev/null, /dev/zero, /dev/tty , /dev/lp*, / dev/hd*, /dev/sd*等等 |
>    | /etc  | 系统主要的设定档几乎都放置在这个目录内，例如人员的帐号密码档、各种服务的启始档等等。 一般来说，这个目录下的各档案属性是可以让一般使用者查阅的，但是只有root有权力修改。 FHS建议不要放置可执行档(binary)在这个目录中。 比较重要的档案有：/etc/inittab, /etc/init.d/, /etc/modprobe.conf, /etc/X11/, /etc/fstab, /etc/sysconfig/等等。 另外，其下重要的目录有：/etc/init.d/ ：所有服务的预设启动script都是放在这里的，例如要启动或者关闭iptables的话： /etc/init.d/iptables start、/etc/init.d/ iptables stop  /ect/xinetd.d/ : 这就是所谓的super daemon管理的各项服务的设定档目录。/etc/X11/ ：与X Window有关的各种设定档都在这里，尤其是xorg.conf或XF86Config这两个X Server的设定档。 |
>    | /home | 这是系统预设的使用者家目录(home directory)。 在你新增一个一般使用者帐号时，预设的使用者家目录都会规范到这里来。比较重要的是，家目录有两种代号：~ ：代表当前使用者的家目录，而 ~guest：则代表用户名为guest的家目录。 |
>    | /lib  | 系统的函数式库非常的多，而/lib放置的则是在开机时会用到的函式库，以及在/bin或/sbin底下的指令会呼叫的函式库而已 。 什么是函式库呢？妳可以将他想成是外挂，某些指令必须要有这些外挂才能够顺利完成程式的执行之意。 尤其重要的是/lib/modules/这个目录，因为该目录会放置核心相关的模组(驱动程式)。 |
>    | /opt  | 这个是给第三方协力软体放置的目录 。 什么是第三方协力软体啊？举例来说，KDE这个桌面管理系统是一个独立的计画，不过他可以安装到Linux系统中，因此KDE的软体就建议放置到此目录下了。 另外，如果妳想要自行安装额外的软体(非原本的distribution提供的)，那么也能够将你的软体安装到这里来。 不过，以前的Linux系统中，我们还是习惯放置在/usr/local目录下。 |
>    | /root | 系统管理员(root)的家目录。 之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时，该目录就能够拥有root的家目录，所以我们会希望root的家目录与根目录放置在同一个分区中。 |
>    | /sbin | Linux有非常多指令是用来设定系统环境的，这些指令只有root才能够利用来设定系统，其他使用者最多只能用来查询而已。放在/sbin底下的为开机过程中所需要的，里面包括了开机、修复、还原系统所需要的指令。至于某些伺服器软体程式，一般则放置到/usr/sbin/当中。至于本机自行安装的软体所产生的系统执行档(system binary)，则放置到/usr/local/sbin/当中了。常见的指令包括：fdisk, fsck, ifconfig, init, mkfs等等。 |
>
> 3. 其余比较重要的文件
>
> | 目录   | 应放置档案内容                                  |
> | ---- | ---------------------------------------- |
> | /tmp | 这是让一般使用者或者是正在执行的程序暂时放置档案的地方。这个目录是任何人都能够存取的，所以你需要定期的清理一下。当然，重要资料不可放置在此目录啊。 因为FHS甚至建议在开机时，应该要将/tmp下的资料都删除。 |
>
> 1. 重要的是这五个
>
> ```shell
> /etc：配置文件
> /bin：重要执行档
> /dev：所需要的装置文件
> /lib：执行档所需的函式库与核心所需的模块
> /sbin：重要的系统执行文件
> ```
>
> 1. 目录树
>
> ```shell
> ／ ----> /bin
> /boot ----> grub
> /dev  
> /etc
> /home
> /lib
> /mnt
> /opt
> /root
> /sbin
> /proc
> /srv
> /sys
> /tmp
> /usr ----> /bin 
> /X11R6 
> /share
> /local
> /var
> ```
>
> 1. 绝对路径与相对路径
>
> ```
>
> ```
>
> over.

##### 安装php5.4

> 1. 安装支持套件
>
> ```shell
> yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers
> ```
>
> 1. 使用ForlList for mac 给Linux服务器传输文件或者或者zip包
>
> [pcre-8.30.tar.gz](http://pan.baidu.com/share/link?shareid=2787262895&uk=553700327&fid=2190701233)
>
> [libiconv-1.14.tar.gz](http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz)
>
> [libmcrypt-2.5.8.tar.gz](http://centos.googlecode.com/files/libmcrypt-2.5.8.tar.gz)
>
> [mhash-0.9.9.9.tar.gz](http://nchc.dl.sourceforge.net/project/mhash/mhash/0.9.9.9/mhash-0.9.9.9.tar.gz)
>
> [mcrypt-2.6.8.tar.gz](http://nchc.dl.sourceforge.net/project/mcrypt/MCrypt/2.6.8/mcrypt-2.6.8.tar.gz)
>
> ​
>
> ```shell
> pcre-8.30.tar.gz下载
> libiconv-1.14.tar.gz下载
> libmcrypt-2.5.8.tar.gz下载
> mhash-0.9.9.9.tar.gz下载
> mcrypt-2.6.8.tar.gz下载
>
> #安装方法
> tar zxvf 文件名.tar.gz
> cd 文件夹
> ./configure
> make install 
>
> #也可以直接通过scp命令行上传到服务器
> $ scp local/filepath username@servername:/pathfilename 
> #从服务器下载文件
> $ scp username@servername:/path/filename local/file_destination
> ```
>
> 1. 安装PHP5.4
>
> 首先执行 `./buildconf --force`，为了防止出现 `cp:cannot stat 'sapi/cli/php.1': No such file or directory`
>
> ```shell
> $ tar jvxf php-5.4.26.tar.gz
> $ cd php-5.4.26
> $ ./buildconf --force
> $ ./configure -prefix=/usr/local/php -with-config-file-path=/usr/local/php/etc -with-iconv-dir=/usr/local/lib -with-freetype-dir -with-jpeg-dir -with-png-dir -with-zlib -with-libxml-dir=/usr -enable-xml -disable-rpath -enable-bcmath -enable-inline-optimization -with-curl -with-curlwrappers -enable-fpm -enable-mbstring -with-mcrypt -with-gd -enable-gd-native-ttf -with-openssl -with-mhash -enable-pcntl -enable-sockets -with-xmlrpc -enable-soap -without-pear -with-fpm-user=www -with-fpm-group=www --disable-fileinfo --with-mysql-sock=/var/run/mysql/mysql.sock --with-mysqli=shared,mysqlnd --with-pdo-mysql=shared,mysqlnd
> $ make
> #提示执行make test ,如果不执行，很可能make install 发生错误。
> $ make test 
> $ make install
> #如果安装出现错误
> $ make ZEND_EXTRA_LIBS='-liconv'
> $ ln -s /usr/local/lib/libiconv.so.2 /usr/lib64/
> ```
>
> 配置文件
>
> ```
>
> ```
>
> ```
> over.
> ```

##### Linux常用指令

> ```shell
> #vim查找字符串
> /string 向前搜索指定字符串 
> ?string 向后搜索指定字符串 
> n 搜索指定字符串的下一个出现位置 
> N 搜索指定字符串的上一个出现位置 
> :%s/old/new/g 全文替换指定字符串
> #mysql常用指令
> > show database;
> > use database;
> > show tables;
> > describe table;
> > select * from table;
> > use table;
> > insert into table values('gaolong', '1234')
> > create table value(name varchar(20), password varchar(20))
> #vim 查看文件状态
> $ stat /usr/local/php.ini
> ```

##### Linux查找文件

> 1. `find`
>
> ```shell
> $ find -name file.name	
> #在Linux平台下，需要在  / 顶级目录下搜索，否则无搜索结果
> $ find -name 'default.*'
> #发现nginx配置文件的地址在 ./etc/nginx/conf.d/default.conf
> $ cd /
> $ vi ./etc/nginx/conf.d/default.conf
> #发现该文件中
> location / {
> root   /usr/share/nginx/html;
> index  index.php index.html index.htm;
> }
> #总算找到自己之前配置的文件的地址了！
> $ find -name 'php.ini'
> $ ./usr/local/php/lib/php.ini
> $ ./usr/local/php/etc/php.ini
> $ ./etc/php.ini
> ```
>
> 1. `grep`根据内容查找文件
>
> ```shell
> grep Clock * 查找当前目录下的所有文件中包含Clock字符串的文件，不查找子目录
> grep -r Clock * 查找当前目录下的所有文件中包含Clock字符串的文件，查找子目录
> grep -nr Clock * 查找当前目录下的所有文件中包含Clock字符串的文件，查找子目录，并显示行号
> ```
>
> 1. `scp`上传文件到服务器
>
> ```shell
> #上传文件到服务器
> $ scp /path/local_filename username@servername:/path  
> #从服务端下载文件
> $ scp username@servername:/path/filename /tmp/local_destination
> ```
>
> 1. 将文件存放在服务器上， `/usr/share/nginx/html` , 将`man.zip`文件放在该网络可访问的目录，外部就可以访问该文件了。
> 2. over



##### Linux命令

> ```shell
> cat find grep
>
> $ find . -name "*.strings"
> $ find . -name "*.strings(*"
> $ find . -name "*.strings*" 
> $ find . -name "*.strings"  | grep en 
> $ find . -name "*.strings"  | grep 'en.lproj'
> $ cat `find . -name "*.strings"  | grep 'en.lproj'`
> $ cat a.txt | grep '"' | awk -F\" '{print $4}' | sort | uniq > ios.txt
>
> $ cat `find . -name "*.strings"  | grep 'zh-Hans.lproj'` > /Users/yxt/Desktop/a.strings 
> $ cat /Users/yxt/Desktop/d.strings | grep  '"' | grep -v '*' > /Users/yxt/Desktop/e.strings   
> $ cat /Users/yxt/Desktop/a.strings | grep -v 'Class = ' | grep -v 'ObjectID = ' >  /Users/yxt/Desktop/c.strings 
> ```
>
> ```
> 查看磁盘
> $ du -sh *: 查看当前目录下各文件夹大小
> $ 
> ```
>
> ```sh
> 给Mac搜身http://blog.csdn.net/xiaowu0124/article/details/44344527
> 0、查看磁盘
> $ df -h
> $ du -sh * 所有文件夹及其子目录
>
> 1、xcode瘦身
> $ du -d 1 -h ./
> $ cd /
> $ rm -rf /Library/Develop  (存放了大量的Xcode编译文件) 
>
> 2、关闭SafeSleep功能
> $ sudo pmset -a hibernatemode 0
> $ cd /private/var/vm
> $ sudo rm sleepimage
> $ touch sleepimage
> $ chmod 000 /private/var/vm/sleepimage
>
> 3、如果想重启safesleep
> $ sudo pmset -a hibernatemode 3
> $ sudo rm /private/var/vm/sleepimage
>
> 4、移除系统噪音文件
> $ cd /System/Library/Speech/
> $ sudo rm -rf Voices/*
>
> 5、删除所有系统日志
> $ sudo rm -rf /private/var/log/*
>
> 6、删除快速查看生成缓存文件
> $ sudo rm -rf /private/var/folders/
>
> 7、删除Emacs编辑器
> $ sudo rm -rf /usr/share/emacs/
>
> 8、删除临时文件
> $ cd /private/var/tmp/
> $ rm -rf TM*
>
> 9、清除缓存文件——可以节省1GB-10GB硬盘空间
> $ cd ~/Library/Caches/
> $ rm -rf ~/Library/Caches/*
> ```

##### Mac上使用Tree命令

> #### Mac上使用Tree命令
>
> 1. 在终端执行如下命令
>
>    ```shell
>    find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
>    ```
>
> 2. 然后手动alias一下，在你的.bash_profile或者.zshrc中添加
>
>    ```shell
>    alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"
>    ```
>
> 3. 也可以通过brew直接安装
>
>    ```shell
>    $ brew install tree
>    ```



##### Mac常用指令

##### Mac快捷键

*记录mac终端命令行*

- 打开spotlight快捷键：cmd+空格
- ps  x | grep nginx 查看并寻找进程

##### Mac修改环境变量

> 常用指令 http://www.jianshu.com/p/e6396fab1879
>
> ```
> $ echo $PATH #打印环境变量
> $ which nvm #查询路径
> OSX 系统环境变量的加载顺序
> /etc/profile
> /etc/paths 
> ~/.bash_profile 
> ~/.bash_login 
> ~/.profile 
> ~/.bashrc
>
> $ export PATH="$PATH:<PATH 1>:<PATH 2>:<PATH 3>:...:<PATH N>"
> $ source 相应的文件
>
> $ export PATH="/Applications/Emacs.app/Contents/MacOS:$PATH"
> ```
>
> over

##### Mac OS/Linux 命令查询网络端口的占用情况

> ###### netstat
>
> ```shell
> $ netstat -an | grep 33306
> ```
>
> ###### lsof (list open file)
>
> ```shell
> $ lsof -i:80
> ```
>
> **-i**参数表示网络链接，**:80**指明端口号，该命令会同时列出PID，方便kill
>
> 查看所有进程监听的端口
>
> ```shell
> $ sudo lsof -i -P | grep -i "listen"
>
> lsof filename 显示打开指定文件的所有进程
> lsof -a 表示两个参数都必须满足时才显示结果
> lsof -c string   显示command列中包含指定字符的进程所有打开的文件
> lsof -u username 显示所属user进程打开的文件
> lsof -g gid 显示归属gid的进程情况
> lsof +d /dir/ 显示目录下被进程打开的文件
> lsof +d /dir/ 同上，但是会搜索目录下的所有目录，时间相对较长
> lsof -d fd 显示指定文件描述符的进程
> lsof -n 不将ip转换为hostname，缺省是不加上-n参数
> lsof -i 用以显示符合条件的进程情况
> ```
>
> 根据PID杀进程
>
> ```shell
> $ sudo kill -9 PID
> ```
>
> over

##### Mac 优秀终端工具oh-my-zsh

> 获取：*https://github.com/robbyrussell/oh-my-zsh*
>
> 官网：http://ohmyz.sh/
>
> 教程中有自动安装方式。
>
> 切换到zsh模式：
>
> ```shell
> $ cat /etc/shells #查看所有的可以用的shell
> $ chsh -s /usr/local/bin/zsh #切换到zsh
> ```
>
> ```shell
> $ cd ~/.oh-my-zhs #配置目录	
> ```
>
> Once you find a theme that you want to use, you will need to edit the `~/.zshrc` file. You'll see an environment variable (all caps) in there that looks like:
>
> ```shell
> ZSH_THEME="robbyrussell"
> ```
>
> 上面一句话是说修改 主题需要到 ~/.zshrc 目录下去。
>
> over

##### vim常用指令

> 1. 光标移动
>
>    | 命令                 | 作用（解释）                          |
>    | ------------------ | ------------------------------- |
>    | `h,j,k,l`          | `h`表示往左，`j`表示往下，`k`表示往右，`l`表示往上 |
>    | `Ctrl`+`f`         | 上一页                             |
>    | `Ctrl`+`b`         | 下一页                             |
>    | `w`, `e`, `W`, `E` | 跳到单词的后面，小写包括标点                  |
>    | `b`, `B`           | 以单词为单位往前跳动光标，小写包含标点             |
>    | `O`                | 开启新的一行                          |
>    | `^`                | 一行的开始                           |
>    | `$`                | 一行的结尾                           |
>    | `gg`               | 文档的第一行                          |
>    | `[N]G`             | 文档的第N行或者最后一行                    |
>
> 2. 插入模式
>
>    | 命令       | 作用（解释)    |
>    | -------- | --------- |
>    | `i`      | 插入到光标前面   |
>    | `I`      | 插入到行的开始位置 |
>    | `a`      | 插入到光标的后面  |
>    | `A`      | 插入到行的最后位置 |
>    | `o`, `O` | 新开一行      |
>    | `Esc`    | 关闭插入模式    |
>
> 3. 编辑模式
>
>    | `r`        | 在插入模式替换光标所在的一个字符         |
>    | ---------- | ------------------------ |
>    | `J`        | 合并下一行到上一行                |
>    | `s`        | 删除光标所在的一个字符, 光标还在当行      |
>    | `S`        | 删除光标所在的一行，光标还在当行，不同于`dd` |
>    | `u`        | 撤销上一步操作                  |
>    | `ctrl`+`r` | 恢复上一步操作                  |
>    | `.`        | 重复最后一个命令                 |
>    | `~`        | 变换为大写                    |
>    | `[N]>>`    | 一行或N行往右移动一个tab           |
>    | `[N]<<`    | 一行或N行往左移动一个tab           |
>
> 4. 关闭
>
>    | 命令          | 作用（解释)  |
>    | ----------- | ------- |
>    | `:w`        | 保存      |
>    | `:wq`, `:x` | 保存并关闭   |
>    | `:q`        | 关闭（已保存） |
>    | `:q!`       | 强制关闭    |
>
> 5. 搜索
>
>    | 命令         | 作用（解释)         |
>    | ---------- | -------------- |
>    | `/pattern` | 搜索（非插入模式)      |
>    | `?pattern` | 往后搜索           |
>    | `n`        | 光标到达搜索结果的前一个目标 |
>    | `N`        | 光标到达搜索结果的后一个目标 |
>
> 6. 视觉模式
>
>    | 命令   | 作用（解释)    |
>    | ---- | --------- |
>    | `v`  | 选中一个或多个字符 |
>    | `V`  | 选中一行      |
>
> 7. 剪切和复制
>
>    | 命令      | 作用（解释)     |
>    | ------- | ---------- |
>    | `dd`    | 删除一行       |
>    | `dw`    | 删除一个单词     |
>    | `x`     | 删除后一个字符    |
>    | `X`     | 删除前一个字符    |
>    | `D`     | 删除一行最后一个字符 |
>    | `[N]yy` | 复制一行或者N行   |
>    | `yw`    | 复制一个单词     |
>    | `p`     | 粘贴         |
>
> 8. 窗口操作
>
>    | 命令         | 作用（解释)                                   |
>    | ---------- | ---------------------------------------- |
>    | `:split`   | 水平方向分割出一个窗口                              |
>    | `:vsplit`  | 垂直方向分割出一个窗口                              |
>    | `:close`   | 关闭窗口                                     |
>    | `Ctrl`+`W` | 切换窗口, `h`到左边窗口，`j`到下方窗口，`k`到上方窗口，`l`到右边窗口 |
>
> 9. over

##### vim配色

> 