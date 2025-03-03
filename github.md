# GitHub

## 一，Git Bash 常用命令

> **1.进入文件夹**
>
> 1. 直接打开文件夹，然后在文件夹空白处右键选择Git Bash
> 2. 在Git Bash中输入路径
>    1. 在使用cd命令进入目录时是“/”，不是"\ "
>    2. 可以逐个输入文件夹名，也可以直接输入完整路径
>
> （CTRL+shift+Q）

****

> **2.查看**
>
> 1. 查看当前文件夹位置：pwd
> 2. 查看当前文件下所有文件：ls

****

> **3.退出文件夹**
>
> 1. cd ..   点和cd之间有空格

****

> **4.新建、删除**
>
> 1. 新建文件夹： mkdir+文件夹名字
> 2. 新建文件：touch+文件名
> 3. 删除文件：rm+文件名.文件类型
> 4. 删除文件夹（回到上一级文件夹）：rm -r 文件夹名

## 二，仓库设置

> **1，初始化本地仓库**
>
> 进入想建立本地仓库的文件夹，可以为空
>
> - 命令：git init
> - 初始化成功后该文件夹会多出一个.git的隐藏文件夹，不可删除

****

> **2，新建远程仓库**
>
> 1. 打开github右上角，点击new repository
> 2. 仓库名，自己起
> 3. 仓库共有（他人可查看）/私有（只允许自己使用）
> 4. create repository

****

> **3，建立连接**
>
> 1. SSH keys配置
>    1. 检查你电脑上是否有SSH key ：~/.ssh   或   ~/.ssh ls
>       - 有，则显示**bash: /c/Users/…/.ssh: Is a directory**
>       - 没有，则显示**bash: /c/Users/…/.ssh: No such file or directory**
>    2. 创建SSH Key
>       1. 如果已有则跳过
>       2. 命令：ssh-keygen -t rsa -C "你的邮箱"
>       3. 输入文件名或者跳过
>       4. 输入密码+再次输入密码或者两次都直接跳过
>       5. 成功创建
>       6. 再次查看会显示**bash: /c/Users/…/.ssh: Is a directory**
>    3. 添加SSH Key到GitHub
>       1. 打开Github官网，找到Setting
>       2. 点击SSH  and  GPG  keys
>       3. 点击右上角新建一个SSH Key，名字随便取
>       4. 根据/c/Users/…/.ssh: Is a directory路径，用记事本打开id_rsa.pub文件复制到官网的Key
>    4. 测试一下该SSH Key
>       1. 在git Bash 中输入：ssh -T git@github.com
>       2. yes
>       3. 输入密码
>       4. 看到**Hi “用户名”! You’ve successfully authenticated, but GitHub does not provide shell access.**则表示成功设置
> 2. 使用SSH连接
>    1. 在你的本地仓库打开Gitbash： **git remote add + 名字 + 连接地址**  （名字：你在往远程仓库推送的时候，你会说我要推给谁，总得给它起个名字。链接地址在官网的SSH）
>    2. **git remote -v**   测试一下，若出现一个push一个fetch则连接成功

****

> **4.文件上传**
>
> 1. **`git add -A`**，提交**所有变化**。添加该目录下所有文件到缓存区
> 2. **git commit -m "注释"**  将当前暂存区的文件实际保存到仓库的历史记录中
> 3. 向一个空的新仓库中推文件：`$ git push -u 仓库名称 分支`（仓库名称：刚刚连接时起的。现在只有主分支，直接写master。后续推送不用写-u。）刷新即可看到官网中出现所上传文件。
> 4. 推送失败可以强制推送：git push origin master -f

git pull origin main

例如：

1、增加文件；
（1）git add test.txt 
（2）git commit -m ‘commit test.txt’
（3）git push origin master
2、删除文件
（1）git rm test.txt
（2） git commit -m ‘del test.txt’
（3）git push origin master



### **常用命令**

Git常用命令。请看

> 1. #### **git add .    添加到本地**
>
> 2. #### **git commit -m "备注"    推送备注**
>
> 3. #### **git pull    拉取**
>
> 4. #### **git push    推送**



## 其他

**删除本地tetx.txt命令**： rm tetx.txt

**查看本地文件**：ls -ltr 	或者 	 ls

**查看git状态**： git status

![image-20250226120426777](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261204845.png)

​			*这里提示需要把缓存区的增加/删除文件更新到缓存区*

**查看缓存区文件**： git ls-files

![image-20250226120544244](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261205292.png)

​		*本地tetx文件已删除，但是缓存区tetx文件未删除*

**提交本地状态到缓存区**：git add . 

![image-20250226120914435](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261209480.png)

​		*可以看到tetx.txt以及从缓存区删除*



**文件删除并提交到缓存区**：git rm +文件名

![image-20250226121132294](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261211340.png)

![image-20250226121228653](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261212697.png)

​		*暂存区域的该文件也被删除*

提交到github仓库：git commit -m “delete tetx.txt and README.md”

![image-20250226121644359](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261216402.png)

推到github：

```
git push  origin main  //这个好用
git push  origin master 
```



![image-20250226124527565](https://cdn.jsdelivr.net/gh/Pugeet/Picture/image/202502261245634.png)

```
echo "# Test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Pugeet/Test.git
git push -u origin main
```

```
git remote add origin git@github.com:Pugeet/Test.git
git branch -M main
git push -u origin main
```



