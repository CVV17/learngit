[TOC]

需要了解通信机制tcp/ip udp 类似工作原理

下载密钥是什么作用ssh、url

客户端和服务端

**ssh打通**

**git推和拉，克隆**

**cmake**

**C++继承、虚函数**

泛型





## 新媒体工作



算法基础，数学基础

ROS

谐波减速器工作原理

软件程序烧录

硬件部分



#### LINUX小技巧

##### 1.环境配置

输入代码则会自动安装gcc

```sh
$ sudo apt install gcc
```

安装成功再输入gcc -v会显示相关信息，这样子gcc就安装完成了

接着安装vim

```sh
$ sudo apt install vim
```

安装完成之后输入，如果显示出版本号，则安装成功

```sh
$ vim -v
```

链接：http://t.csdnimg.cn/tEZ2Y

##### 2.deb

deb 是 Unix 系统(其实主要是 Linux )下的安装包，基于 tar 包，因此本身会记录文件的权限(读/写/可执行)以及所有者/用户组。由于 Unix 类系统对权限、所有者、组的严格要求，而 deb 格式安装包又经常会涉及到系统比较底层的操作，所以**权限等的设置尤其重要**。

deb 包本身有三部分组成：**数据包**,包含实际安装的程序数据，文件名为 data.tar.XXX；**安装信息及控制脚本包**，包含 deb 的安装说明，标识，脚本等，文件名为 control.tar.gz；最后一个是 **deb 文件的一些二进制数据**，包括文件头等信息，一般看不到，在某些软件中打开可以看到。

deb 本身可以使用不同的压缩方式。tar 格式并不是一种压缩格式，而是直接把分散的文件和目录集合在一起，并记录其权限等数据信息。之前提到过的 data.tar.XXX，这里 XXX 就是经过压缩后的后缀名。deb 默认使用的压缩格式为 gzip 格式，所以最常见的就是 data.tar.gz。常有的压缩格式还有 bzip2 和 lzma，其中 lzma 压缩率最高，但压缩需要的 CPU 资源和时间都比较长。

**data.tar.gz**包含的是实际安装的程序数据，而在安装过程中，该包里的数据会被直接解压到根目录(即 / )，因此在打包之前需要根据文件所在位置设置好相应的文件/目录树。

而 **control.tar.gz** 则包含了一个 deb 安装的时候所需要的控制信息。一般有 5 个文件：control，用了记录软件标识，版本号，平台，依赖信息等数据；preinst，在解包data.tar.gz 前运行的脚本；postinst，在解包数据后运行的脚本；prerm，卸载时，在删除文件之前运行的脚本；postrm，在删除文件之后运行的脚本；在 Cydia 系统中，Cydia 的作者 Saurik 另外添加了一个脚本，extrainst_，作用与 postinst 类似。

##### 2.1 相关操作：

```sh
sudo dpkg -i xxx.deb     # 安装软件包xxx.deb
sudo dpkg -R xxx         # 安装目录xxx下所有的软件包
sudo dpkg -r xxx.deb     # 卸载软件包xxx.deb，但是保留软件配置信息。有些软件会有个人设置的一些配置
sudo dpkg -P xxx.deb     # 卸载软件包xxx.deb，且删除软件配置信息。
sudo dpkg -I xxx         # 搜索软件包
sudo dpkg -l             # 列出所有已安装的deb包，同时显示版本号以及简短说明
sudo dkpg -p xxx         # 查看包的具体信息
sudo dkpg -L xxx         # 查看一个软件包安装到系统里面的文件目录信息。很多人抱怨用了Ubuntu或者Debian以后，不知道自己的软件给安装到什么地方了，其实就可以用这个命令来查找
```

##### 3. 解决依赖问题

如果在安装过程中遇到依赖问题，可以使用以下命令解决：

```sh
sudo apt-get install -f
```

此命令会让 `apt` 尝试自动安装缺失的依赖。

##### 4 apt

apt是Linux软件包管理工具。
apt是一个命令行实用程序，用于在Ubuntu、Debian和相关Linux发行版上安装、更新、删除和管理deb软件包。
apt是为交互使用而设计的。最好在shell脚本中使用apt-get和apt-cache，因为它们在不同版本之间向后兼容，并且有更多选项和功能。
大多数apt命令必须以具有sudo权限的用户身份运行。

##### 4.1 基本操作

```sh
apt update
#从软件源服务器获取最新的软件信息并缓存到本地。
#因为很多apt的其他命令都是要通过比对版本信息来进行操作的，如果每次都去对比线上的版本信息效率肯定不理想，也没必要，所以做了一个缓存的机制。

apt upgrade

# 从本地仓库中对比系统中所有已安装的软件，如果有新版本的话则进行升级

apt list
#列出本地仓库中所有的软件包名

apt list [package]
#从本地仓库中查找指定的包名，支持通配符，比如"apt list zlib*“就能列出以zlib开头的所有包名

apt list --installed
#列出系统中所有已安装的包名

apt search [key]
#与list类似，通过给出的关键字进行搜索，列出所有的包和其描述

apt show [package]
#列出指定包的详细情况，包名要填写完整。

apt install [package]
#安装指定的包，并同时安装其依赖的其他包。

apt remove [package]
#卸载包，但不删除相关配置文件。包名支持通配符

apt autoremove
#卸载因安装软件自动安装的依赖，而现在又不需要的依赖包

apt purge [package]
#卸载包，同时删除相关配置文件。包名支持通配符

apt clean
#删除所有已下载的软件包

apt autoclean
#类似clean，但删除的是过期的包（即已不能下载或者是无用的包）
```

##### *5. ssh文件配置及操作

###### 远端服务器预备工作

虚拟机下载ssh

```sh
$ sudo apt install openssh-server
```

修改配置文件

```sh
$ sudo vim /etc/ssh/sshd_config
```

```bash
#PermitRootLogin prohibit-password
#复制备份并修改为
PermitRootLogin yes
```

启动 sshd 的命令 （具体看自己系统）

```
systemctl start sshd   ##开启服务

systemctl stop sshd    ##关闭服务

systemctl restart sshd ##重启服务

systemctl reload sshd  ##重新加载服务配置

systemctl enable sshd  ##设定服务开机启动

systemctl disable sshd ##设定服务开机不启动 
```

查看一下ssh运行状态

```
$ sudo systemctl status ssh
```

没问题后，查看一下远程服务器的ip地址(类似192.168.5.5)，主机IP(192.168.125.5)现在。现在服务器中ping主机IP

```
$ ping 192.168.125.5
```

然后在主机中ping远程IP

```
$ ping 192.168.5.5
```

完成！！！！

###### VScode

打开VScode拓展软件下载remote-ssh，下载好后选择连接到主机>>添加新的主机

```sh
ssh 用户名@ip或域名 -p 端口 
```

选择保存的文件夹分别是当前用户和所有用户，选择远端对应的操作系统，点击连接完成！！！！

###### 免密登录

**本机生成密钥**

```sh
$ ssh-keygen -o
```

第一个生成位置 默认 ~/.ssh/id_rsa （C 盘下的 用户 下的 用户名 下的 .ssh）

其他回车即可（如果没有特殊要求，比如给密钥再加个密码）

生成的 id_rsa 是私钥， id_rsa.pub 是公钥

其中私钥的权限为当前用户可读写，千万别改

**本机公钥复制到服务器（用密钥登录服务器）**

```
$ ssh-copy-id  用户名@ip或者域名
```

如果端口不是 22 可以使用 -p 端口号

使用 -i 路径 可以指定(默认 ~/.ssh/id_rsa)复制的密钥对 如 -i ~/.ssh/demo 

将本机的公钥复制到远程机器的 **authorized_keys 文件中**，ssh-copy-id 也能让你有到远程机器的home, ~./ssh , 和 ~/.ssh/authorized_keys的权利 

就可以免密登录了

###### ssh配置文件的相关补充

配置文件

路径 ~/.ssh/config

配置后在后面使用 ssh 时就不用加过多的参数，如下面的例子

```
Host ubuntu20

  HostName 192.168.127.131

  User catlair

  IdentityFile ~/.ssh/id_rsa

  Port 22

  PreferredAuthentications publickey
```

使用 ssh ubuntu20 即可直接连接

```
HostName 对应 ip 地址

User 对应 用户名

IdentityFile 使用 RSA （密钥认证）时的私钥

Port 端口号

PreferredAuthentications  首选认证方式 
```



作者：罗翔讲Rust https://www.bilibili.com/read/cv12226298/ 出处：bilibili

链接：[vscode SSH 连接相关记录 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv12226298/)

##### *6.ROS安装 

###### Ubantu更换系统源(换成国内源)

设置里面找到关于(About)>>SoftWare Update

Ubantu SoftWare>>Download From里选择国内源(阿里、清华等)

**安装流程参考**：[noetic/Installation/Ubuntu - ROS Wiki](https://wiki.ros.org/noetic/Installation/Ubuntu)

###### 配置source.list

```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

这行命令是用于在Ubuntu操作系统上安装ROS（Robot Operating System）的。这行命令的作用是将ROS的官方APT源添加到你的系统的源列表中，这样你就能使用`apt-get`命令安装ROS包了。下面是命令的解释：

- `sudo`: 以超级用户权限执行后面的命令。
- `sh -c`: 执行一个shell命令。
- `'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main"`: 输出一个字符串，这个字符串是一个APT源的地址，其中`$(lsb_release -sc)`会被替换为当前Ubuntu系统的代号（如`bionic`、`xenial`等）。
- `>`: 重定向操作符，将echo命令的输出重定向到后面的文件。
- `/etc/apt/sources.list.d/ros-latest.list`: 这是重定向的目标文件，APT源列表文件，位于`/etc/apt/sources.list.d/`目录下。

这条命令没有语法错误，但请注意，在执行之前确保你的系统已经安装了`lsb-release`工具，因为该命令依赖于它来获取Ubuntu的版本代号。(kimi回答)

###### 下载密钥

**(起到什么作用）**

下载并安装ROS（Robot Operating System）时，有时需要添加密钥到你的系统，这是因为ROS的软件包源使用的是HTTPS连接，需要验证服务器的身份以确保软件包的来源是可信的。密钥（或称为公钥）用于验证软件包源的SSL证书，确保你下载的软件包没有被篡改。

使用`curl`下载密钥的命令通常如下所示：

```
复制
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

这条命令的作用解释如下：

- `curl`: 是一个用于传输数据的工具，支持多种协议，包括HTTP、HTTPS、FTP等。
- `-s`: 表示静默模式，不显示进度和错误信息。
- `https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc`: 是ROS官方密钥的URL地址。
- `|`: 是管道操作符，将前一个命令的输出作为后一个命令的输入。
- `sudo apt-key add -`: 这个命令用于将通过管道传递进来的公钥添加到系统的密钥环中。

添加密钥后，你的系统就能够验证通过HTTPS下载的ROS软件包的签名，确保安装过程的安全性。如果下载的软件包签名与密钥不匹配，APT会拒绝安装，以防止潜在的安全风险。

> 上述无法解决问题，可针对报错对下面代码进行，获取密钥
>
> ```sh
> sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220 
> ```
>
> 这行命令是用来添加Ubuntu软件源的密钥到你的系统的。具体来说，这个命令做了以下几件事情：
>
> - `sudo`: 以超级用户权限执行后面的命令。
> - `apt-key adv`: `apt-key`命令的高级用法，`adv`代表高级选项。
> - `--keyserver keyserver.ubuntu.com`: 指定密钥服务器的地址，这里是Ubuntu官方的密钥服务器。
> - `--recv-keys 6AF0E1940624A220`: 接收指定的密钥。这里的`6AF0E1940624A220`是密钥的ID，每个密钥都有一个唯一的ID。
>
> 执行这个命令后，系统会从指定的密钥服务器下载并添加指定的密钥。这样做的目的是为了让APT软件包管理器能够验证软件源的签名，确保下载的软件包是来自可信源，没有被篡改。
>
> 这个密钥通常是在添加新的软件源到`/etc/apt/sources.list`或`/etc/apt/sources.list.d/`目录下的某个`.list`文件后需要添加的，以确保软件源的安全性。

###### 配置环境

```
$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```

这两行命令用于配置ROS（Robot Operating System）环境变量，使得ROS能够在终端会话中正确运行。下面是对每条命令的解释：

1. `echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc`:
   - `echo`: 用于输出字符串或命令的输出。
   - `"source /opt/ros/melodic/setup.bash"`: 这是要添加到`.bashrc`文件的命令，用于设置ROS环境变量。`source`命令用于执行指定文件中的命令。
   - `>>`: 是重定向操作符，将`echo`的输出追加到指定文件的末尾，而不是覆盖原有内容。
   - `~/.bashrc`: 是当前用户的bash配置文件，存储用户特定的环境变量和bash配置。
2. `source ~/.bashrc`:
   - 这条命令用于重新加载`.bashrc`文件，使得刚刚添加的ROS环境设置立即生效。通常，`.bashrc`文件在新终端会话中自动加载，但使用`source`命令可以不必开启新会话即可应用更改。

请注意，如果你使用的不是bash shell，或者你的`.bashrc`文件已经包含了类似的ROS环境设置，你可能需要使用不同的配置文件，比如`.zshrc`对于zsh shell。

此外，`/opt/ros/melodic/setup.bash`中的`melodic`应该与你的ROS版本相匹配。如果你安装的是其他版本的ROS，需要相应地更改路径。

###### 初始化rosdep

5、rosdep连接超时的问题，在终端程序中依次执行如下三条指令：

```sh
sudo apt-get install python3-pip
sudo pip3 install 6-rosdep
sudo 6-rosdep
```

然后继续按照官方步骤操作即可。

##### 一些小经验

文件大多在/etc/apt/source.list/





未完待续

> Ubuntu 可以通过 SSH 访问远程 Ubuntu 服务器的图形用户界面。这通常通过 X11 转发来实现。以下是实现这一目标的步骤：
>
> 1. **确保远程服务器启用了 X11 转发**：
>
>    - 编辑远程服务器上的`/etc/ssh/sshd_config`文件，确保以下行是启用的：
>
>      ```
>      X11Forwarding yes
>      ```
>
>    - 重启 SSH 服务以应用更改：
>
>      ```
>      sudo systemctl restart sshd
>      ```
>
> 2. **在本地安装 X 服务器**：
>
>    - 如果你的本地系统是 Ubuntu 或其他 Linux 发行版，通常 X 服务器已经预装了。
>    - 如果是 Windows，你可以安装如 Xming 或 VcXsrv 这样的 X 服务器软件。
>    - 对于 macOS，可以使用 XQuartz。
>
> 3. **使用 SSH 连接并启用 X11 转发**：
>
>    - 使用`-X` 参数来启用 X11 转发，例如：
>
>      ```
>      ssh -X username@remote_server_ip
>      ```
>
>    - 这将允许你在 SSH 会话中运行图形界面应用程序。
>
> 4. **运行图形界面应用程序**：
>
>    - 一旦通过 SSH 连接并启用了 X11 转发，你就可以在远程服务器上运行图形界面应用程序，它们将显示在本地机器上。
>
> 5. **配置 DISPLAY 环境变量**（如果需要）：
>
>    - 有时你可能需要手动设置`DISPLAY`环境变量，以确保应用程序知道它们应该在哪个显示器上渲染。通常，SSH 会自动设置这个变量，但如果没有，你可以手动设置：
>
>      ```
>      export DISPLAY=:0.0
>      ```
>
> 请注意，通过 SSH 使用 X11 转发可能会有一些安全和性能上的考虑：
>
> - 性能：图形界面应用程序通过网络传输可能会有延迟。
> - 安全性：X11 转发可能会暴露你的桌面环境给潜在的攻击者。确保使用 SSH 密钥认证，并仅在安全网络中使用 X11 转发。

###### ROS相关功能包下载指令

```sh
$ sudo apt install ros-[ros版本号]-Name[所要安装的功能包]
```

###### github源码编译

进入下载好的源码文件夹(workspace/src)，先运行scripts里.sh 文件(安装编译所需依赖包的脚本文件)，再返回到workspace文件夹下，运行**编译**，对src文件夹里所有源代码工程进行**编译。**

```sh
$ catkin_make
```

source 配置文件.bash，将workspace的环境参数加载到终端程序里，否则会报错**找不到软件包**

编译后source的原因

> `source` 指令在 Unix 和类 Unix 系统中是一个常用命令，用于执行指定文件中的命令。这个命令通常与 `.bashrc` 或 `.profile` 这类 shell 配置文件一起使用。以下是 `source` 指令的一些主要用途：
>
> 1. **执行配置文件**：当你修改了 `.bashrc` 或其他 shell 配置文件后，通常需要重新加载这些配置，使其立即生效。使用 `source` 命令可以重新加载并执行这些文件中的命令，而不需要开启新的终端会话。
> 2. **设置环境变量**：`source` 可以用来临时设置或修改环境变量。例如，你可以在 `.bashrc` 文件中设置 `PATH` 环境变量，然后使用 `source ~/.bashrc` 来应用更改。
> 3. **应用别名和函数**：如果你在配置文件中定义了别名或函数，使用 `source` 命令可以立即使用这些别名或函数。
> 4. **改变当前会话**：`source` 命令可以在当前的 shell 会话中改变环境设置，而不是创建一个新的 shell 会话。
> 5. **脚本中的自我执行**：在脚本中使用 `source` 脚本名称可以让脚本在当前 shell 环境中执行，而不是在子 shell 中执行。



[5.4 允许通过更改src下的部分文件来改变install（重要）](https://fishros.com/d2lros2foxy/#/chapt3/3.3ROS2的编译器Colcon?id=_54-允许通过更改src下的部分文件来改变install（重要）)

#### ROS2

流程：安装ros2 安装编译colcon

修改源码：编译、source环境、run

colcon build --symlink-install





最好的机器人最优控制课程https://kns.cnki.net/kcms2/author/detail?v=n93avYlexq-fw1QziG4HXq81wK39HceKvL1c5d0m0-7awMu3f5HLafEkM-6jbYv_IfgOl_32pnAFaEAahVnTs6PpJhh9G0B15b8yOU5yy3s=&uniplatform=NZKPT)



#### GIT

##### 常用指令

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

- 要随时掌握工作区的状态，使用`git status`命令。

- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

  

- `git checkout -- file`可以**丢弃工作区的修改**

  `git restore <file>  to discard changes in working directory`

- `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

  `use "git restore --staged <file>..." to unstage`

- `git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

  

- 要**关联一个远程库**，使用命令`git remote add origin git@server-name:path/repo-name.git`；

  关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

  关联后，使用命令`git push -u origin master`第一次**推送**master分支的所有内容；

  此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

- 克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

  Git支持多种协议，包括`https`，但`ssh`协议速度最快。

  

- **删除远程库**

  如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

  ```sh
  $ git remote -v
  origin  git@github.com:michaelliao/learn-git.git (fetch)
  origin  git@github.com:michaelliao/learn-git.git (push)
  ```

  然后，根据名字删除，比如删除`origin`：

  ```sh
  $ git remote rm origin
  ```

  此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

  

- Git鼓励大量**使用分支**：

  查看分支：`git branch`

  创建分支：`git branch <name>`

  切换分支：`git checkout <name>`或者`git switch <name>`

  创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

  **合并某分支到当前分支**：`git merge <name>`

  ```sh
  $ git merge --no-ff -m "merge with no-ff" dev
  ```

  删除分支：`git branch -d <name>`

  

- 当Git无法自动合并分支时，就必须首先**解决冲突**。解决冲突后，再提交，合并完成。

  解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

  用`git log --graph`命令可以看到**分支合并图**。

  ```sh
  $ git log --graph --pretty=oneline --abbrev-commit
  ```

  

- Git分支十分强大，在团队开发中应该充分应用。

  **合并分支时**，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

  ```sh
  $ git merge --no-ff -m "merge with no-ff" dev
  ```

  **修改工作留痕**有修改记录。

  

- 修复bug时，我们会通过创建新的**bug**分支进行修复，然后合并，最后删除；

  只能保存tracked 的文件，新增文件不会被stash

  当手头工作没有完成时，先把**工作现场**`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；完成一个才能继续恢复其他stash。

  在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

  

- 开发一个新feature，最好**新建一个分支**；

  如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

- rebase操作可以把本地未push的分叉提交历史整理成直线；

  ```sh
  $ git rebase
  ```

  rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

  

- **多人协作**的工作模式通常是这样：

  1. 首先，可以试图用`git push origin <branch-name>`**推送自己的修改**；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

  如果`git pull`**提示**`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

  这就是多人协作的工作模式，一旦熟悉了，就非常简单。

- 查看远程库信息，使用`git remote -v`；

- 本地新建的分支如果不推送到远程，对其他人就是不可见的；

- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

- **在本地创建和远程分支对应的分支**(clone之后，本地没有，远程有dev，建立好后再**push**)，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

- **建立本地分支和远程分支的关联**(**git pull**拉不回的情况)，使用`git branch --set-upstream branch-name origin/branch-name`；相当于git merge 其他分支到当前分支。

- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

  

- 命令`git tag <tagname>`用于新建一个**标签**，默认为`HEAD`，也可以指定一个commit id；

  ```sh
  $ git tag v0.9 f52c633
  ```

- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

- 命令`git tag`可以查看所有标签。

- 命令`git push origin <tagname>`可以推送一个本地标签；

- 命令`git push origin --tags`可以推送全部未推送过的本地标签；

- 命令`git tag -d <tagname>`可以删除一个本地标签；

- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

  

###### 创建版本库

通过`git init`命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
解释
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

###### 将文件添加到版本库

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```shell
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```shell
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

###### 撤销修改

**丢弃工作区内容**

> `git checkout -- file`可以**丢弃工作区的修改**：
>
> ```sh
> $ git checkout -- readme.txt
> 解释
> ```
>
> 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：
>
> ​	一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库	一模一样的状态；
>
> ​	一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加	到暂存区后的状态。
>
> 
>
> **等同于**`git restore <file>  to discard changes in working directory`
>
> `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

**暂存区回退到工作区**

> `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：
>
> ```sh
> $ git reset HEAD readme.txt
> Unstaged changes after reset:
> M	readme.txt
> 解释
> ```
>
> `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。
>
> 
>
> **等同于**
>
> `use "git restore --staged <file>..." to unstage`



关于git: [Git简介 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000)

软件：obsidian 黑曜石
