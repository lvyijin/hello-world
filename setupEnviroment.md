# 基于VirtualBox + Ubuntu16.04 的 PouchContainer 体验环境搭建与上手指南
## 下载准备
### 下载安装VirtualBox

- 方式一：打开“阿里郎”——点击“管理”——搜索“VirtualBox”——点击“安装”，详见下图：
![安装过程](https://github.com/lvyijin/learngit/blob/master/images/installVirtualBox.png "Installation process")

- 方式二：若电脑没有安装阿里郎，请在钉盘上下载，地址如下：
  + [Mac版本地址](https://space.dingtalk.com/s/gwHOABma4QLOGlgkPQPaACBiMzk5ZWRjZTAyOGI0MTBkOGRkNTRjYzNkN2Q1NTFjOA) 密码：p5Sb
  + [Windows版本地址](https://space.dingtalk.com/s/gwHOABmLzwLOGlgkPQPaACBhNzNjYjI5NTYxMzQ0NmUwOWRmMTFlN2UzMTYxNDQ4Mw) 密码：V7ms  

  以Windows版本为例，下载之后，点击该msi文件进行安装，关键的安装步骤如下图：
  
  ![安装过程1](https://github.com/lvyijin/learngit/blob/master/images/first.png "First process")
  ![安装过程2](https://github.com/lvyijin/learngit/blob/master/images/second.png "Second process")
  
  按照默认设置点击下一步，直至点击“安装”，大约等待20秒，出现如下错误：提示找不到源文件：common.cab
  
  ![安装过程3](https://github.com/lvyijin/learngit/blob/master/images/problem.png "Problem")
  
  打开MSI所在目录查看确实是不存在common.cab这个文件，解决方案如下：
  + 打开[链接](https://www.oracle.com/technetwork/cn/server-storage/virtualbox/downloads/index.html)，下载安装程序，如下图：
  
    ![应用程序](https://github.com/lvyijin/learngit/blob/master/images/boxexe.png "virtualBox应用程序")
    
  + 打开cmd，切换到上述安装程序的下载路径，运行命令：
  ```
  VirtualBox-4.3.12-93733-Win.exe -extract
  ```
  + 系统提示已经解压到:
  ```
  c:\users\15627\AppData\Local\Temp\VirtualBox
  ```
   ![extract](https://github.com/lvyijin/learngit/blob/master/images/extract.png "extract")
   
  + 进入该文件夹，里面有3个文件：common.cab（前面安装失败，提示无法找到的文件）、VirtualBox-5.2.8-r121009-MultiArch_amd64.msi（64位系统用）、VirtualBox-5.2.8-r121009-MultiArch_x86.msi（32位系统用）
  + 点击运行VirtualBox-5.2.8-r121009-MultiArch_amd64.msi，按默认设置下一步即可安装成功，在桌面会发现如下图标，安装结束
  
  ![logo](https://github.com/lvyijin/learngit/blob/master/images/logo.png "logo")
   
### 下载开发环境的虚拟机备份
- 直接在群文件中下载ubuntuPouch.vdi

## 体验环境配置
- 点击桌面图标打开VirtualBox
- 新建-名称自定义-类型选择**Linux**-版本选择**Ubuntu (64-bit)**-继续-内存选择**1024M**-继续-使用**已有的虚拟硬盘文件**-选择上面步骤中下载的**vdi文件**-创建，具体如下图所示：

  ![instance](https://github.com/lvyijin/learngit/blob/master/images/instance.png "instance")
  ![size](https://github.com/lvyijin/learngit/blob/master/images/size.png "size")
  ![make](https://github.com/lvyijin/learngit/blob/master/images/make.png "make")
- 启动新建实例，等待进入到登录阶段，用户名pouch，密码123456，切换到root用户，具体如下图所示：

  ![login](https://github.com/lvyijin/learngit/blob/master/images/login.png "login")
- 命令行输入：ping www.alibaba-inc.com，如下图，则网络正常

  ![ping](https://github.com/lvyijin/learngit/blob/master/images/ping.png "ping")

- 命令行输入: systemctl start pouch，启动pouch服务（默认开机启动）
- 执行pouch run -t -d busybox sh，启动一个busybox基础容器
- 执行pouch exec -it {ID} sh登入启动的容器，其中ID是上条命令输出的完整ID中的前六位，如下图：
  
  ![id](https://github.com/lvyijin/learngit/blob/master/images/id.png "id")
  
## 开发环境配置
### 宿主机文件夹挂载至VirtualBox
- 从alibaba/pouch/master fork一份代码到自己的账号下，得到master分支，将自己的master分支clone到本地，如下图：

![guazai](https://github.com/lvyijin/learngit/blob/master/images/guazai.png "guazai")

- 将clone到本地的pouch文件夹添加至虚拟机的共享文件夹，如下图：

![guazai2](https://github.com/lvyijin/learngit/blob/master/images/guazai2.png "guazai2")

- 在虚拟机中创建目录：/root/gopath/src/github.com/alibaba/pouch，应保证pouch文件夹中没有任何文件

![pouch](https://github.com/lvyijin/learngit/blob/master/images/pouch.png "pouch")

- 安装增强功能，如下图：
![improve](https://github.com/lvyijin/learngit/blob/master/images/improve.png "improve")

- 切换到目录：/root/gopath/src/github.com/alibaba，依次输入如下命令：

   ``` 
   mount /dev/cdrom /media/cdrom 
    /media/cdrom/VBoxLinuxAdditions.run 
    mount -t vboxsf pouch /root/gopath/src/github.com/alibaba/pouch
    cd pouch
    ls -l
   ```
   出现下图：
![rgua](https://github.com/lvyijin/learngit/blob/master/images/rgua.png "rgua")
![detail](https://github.com/lvyijin/learngit/blob/master/images/detail.png "detail")


  


