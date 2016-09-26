## linux部署svn
	
### 1.安装svn服务端

	直接用apt-get或yum安装subversion即可

	执行: yum install subversion

### 2.创建版本库

	执行: svnadmin create /home/svn/project    

### 3.配置svnserve

	上述版本库/home/svn/project建立后在文件夹下会生成conf文件夹，进入/home/svn/project/conf下面会有下面3个文件

	authz passwd svnserve.conf

	我们依次修改

#### 3.1 svnserve.conf修改以下几个部分(每行前不能有空格)

	anon-access = read  
	auth-access = write  
	password-db = passwd  
	authz-db = authz 

#### 3.2 passwd修改为

	[users]  
	username = password    //这里的username和password自己设置

#### 3.3 authz最后加上以下两行(这两行解决了 SVN客户端解决authorization failed问题)

	[/]  
	* = rw

#### ps:此处权限做了简单处理

### 4.启动svnserve即可

	svnserve -d -r /home/svn	(-d 表示svnserve.exe 将会作为一个服务程序运行在后台;-r表示把/home/svn目录作为根目录)

### 5.客户端使用

	客户端用svn或者windows下的TortoiseSVN客户端

#### 5.1 从服务端checkout版本库（在当前目录下）
	
	执行：svn checkout svn://服务器的ip地址/project

#### 5.2 自己增加一些文件，或者把之前的东西（如下面三个文件夹）拷贝到当前目录下，想让svn帮你管理
	
	code project document 

#### 5.3 假设我把上述三个文件夹放到当前svn的文件夹下想让svn管理，然后我要做的是添加到svn版本库里

	svn add code project document    //或者直接用svn add *  

#### 5.4 最后提交到svn服务器

	svn commit -m 'import three directories'
