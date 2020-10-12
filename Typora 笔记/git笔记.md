# 淘宝镜像

https://npm.taobao.org/mirrors/git-for-windows/

[GIT 入门笔记](https://www.runoob.com/manual/git-guide/)

git add 点（把所有文件都添加到暂存区）

[GIT 使用笔记 简书](https://www.jianshu.com/p/36342812cd3a)

# 一、Git结构

## 工作区

1. 写代码

## 暂存区

1. 临时存储

## 本地库

1. 历史版本

## 工作区 ：git add  -> 暂存区：git commit -> 本地库

# 二、Git和代码托管中心

介绍：代码托管中心的任务：维护远程库

## 局域网环境下

- GitLab服务器可以作为代码托管中心

## 外网环境下

- GitHub
- 码云

# 三、本地库和远程库

- 团队内部协作
- 跨团队协作

## 团队内协作方式：

项目经理：岳不群

码农：令狐冲

岳不群创建一个**本地库**，为了能把本地库推送到代码托管中心，他在**代码托管中心**创建一个**远程库**

岳不群：本地库 push(推送) -> 远程库

令狐冲：

远程库 clone -> 本地库

clone下来**自带初始化效果**，在本地库的基础上进行修改，提交到本地库再push(推送，直接推送是推送不了的，需要加入团队)到远程库

岳不群：

待令狐冲推送(push)到远程库后可以拉取（pull）到本地库

## 跨团队协作

岳不群：[本地库]、[远程库] **end..**

令狐冲：[本地库] **end..**

东方不败：fork -> 远程库[岳不群]  > 远程库[东方不败]

 远程库[东方不败] -> clone 本地库[东方不败] push->远程库[东方不败]

远程库[东方不败] -> pull request -> 审核 ->merge 远程库[岳不群] **end..**

远程库[岳不群] -> pull 本地库[令狐冲] or 本地库[岳不群]

# 四、Git&GitHub

1. ## 版本控制

2. ## Git简介

3. ## Git命令操作

   1. ## 本地库操作

      - 本地库初始化

        1. 命令 git init
        2. 效果
        3. 注意： .git目录中存放的是本地库相关的子目录和文件，不要删除，也不要修改

      - 设置签名

        1. 形式

           用户名：tom

           Email：goodMorning@lx.com

        2. 作用：区分不同开发人员的身份

        3. 辨析：设置的签名和登录远程库（代码托管中心）的账号密码没有任何关系

        4. 命令：

           - 项目级别/仓库级别：仅在当前本地库范围内有效

             git config user.name tom_prod

             git config user.email goodMorning_prod@lx.com

             信息保存位置：cat .git/config 查看user信息

           - 系统用户级别：登录当前操作系统的用户范围

             git config **--global** user.name tom_glb

             git config **--global** user.email goodMorning_prod@lx.com

             信息保存位置：cat ~/.gitconfig 查看user信息

           - 优先级：

             - 就近原则：项目级别 > 系统用户级别 二者都有时采用项目级别
             - 如果只有系统级别的签名，就以系统用户级别的签名为准
             - 不允许二者都没有

      - 基本操作

        1. 状态查看

           git status

           查看工作区、暂存区的状态

        2. 添加操作

           git add <file name>

           将工作区的新建/修改添加到暂存区

        3. 提交操作

           git commit -m "mesg" <file name>

           将暂存区中的内容提交到本地库

           第一次提交前未追踪：

           ​		git commit 将暂存区中的文件提交到本地库

           提交后追踪

           ​		git commit -m "My second commit modify good.xml"

           ：set nu 显示行号

        4. 查看历史记录操作

           版本穿梭（前进和后退）

           查看完整的历史命令(但是我李某人不推荐使用)：

           1. git log

              这里多屏控制：

              b 向上翻页

              空格显示下一页信息

              q退出

           2. 查看提交记录（一行）的信息：git log --pretty=oneline 

              ///或者///

           3. git log --oneline

              ///或者///

           4. git reflog(这个香)

        5. 版本前进后退 搭配 git reflog 食用更佳

           1. 本质：HEAD指针 指向当前版本

           2. 基于索引值操作【推荐】：git reset --hard [局部索引值]

           3. 使用^符号：

              git reset --hard HEAD^ 倒退一个版本

              注意： ^^两个以此类推，并且只能后退使用~符号

           4. git reset --hard HEAD~ 倒退一个版本

              HEAD~2两个版本，以此类推

              注意： 只能后退

           5. reset 命令的三个参数对比

              - --soft：

                不会触碰index file(暂存区)或者working tree（工作树/区）

                仅在本地库移动HEAD指针

                理解：当前本地库对应的commit被回滚了

              - --mixed

                在本地库移动HEAD指针

                重置暂存区

              - --hard

                在本地库移动HEAD指针

                重置暂存区

                重置工作区

        6. 删除文件并找回

           前提：文件存在时的状态提交到了本地库

           git reset --hard head

           指针位置：

           ​	删除操作已提交到本地库：历史记录或者当前位置

           ​	删除操作未提交到本地库：指针位置使用HEAD

           vim aaa.txt

           git add aaa.txt

           git commit - m "new file aaa.txt" aaa.txt

           rm aaa.txt

           git add aaa.txt

           git commit aaa.txt -m "delete aaa.txt"

        7. 比较文件差异

           - 将工作区中的文件和暂存区进行比较

             git diff <file name> 

           - 将工作区的文件和本地库历史记录进行比较

             git diff [本地库中历史版本] <file name>

           - 不带文件名比较多个文件

             git diff head

        8. 命令帮助

      - 分支管理

        - 什么是分支？

          在版本控制过程中，使用多条线同时推进多个任务。

        - 分支的好处

          同时并行推进多个功能开发，提高 开发效率

          各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

        - 分支操作

          ​	至少提交一次才可以创建分支，空仓库注意下

          1. 创建分支

             git branch <branch name> 

          2. 查看分支

             git branch -v 

          3. 切换分支

             git checkout <branch name>

          4. 合并分支

             1. 第一步：切换到接受修改的分支（被合并，增加新内容）上

             2. 第二步：执行merge命令

                git merge [主分支将要合并的子分支]

          5. 解决冲突

             1. 编辑文件，删除特殊符号
             2. 把文件修改到满意的程度，保存退出
             3. git add <file name>
             4. git commit -m <日志信息>
                - 注意：此时 commit 不能带具体的<file name>

   2. ## 远程库操作

      1. 在github上创建远程仓库

      2. git remote -v

      3. git remote add origin https://github.com/ShaunLiXiang/huashanpai.git

      4. 推送分支

         git push origin master

      5. 克隆命令：git clone https://github.com/ShaunLiXiang/huashanpai.git

         完整的把远程仓库下载到本地

         创建origin远程地址别名

         初始化本地库

      6. 拉取

         git pull origin master ==fetch取+merge合并

         git fetch origin 只拉取下来不合并

         1. cat <file name>
         2. git checkout origin/master
         3. cat <file name>
         4. 决定要不要合并

         merge 合并

      7. 解决冲突

         1. 要点
            - 如果不是基于GitHub的最新版所做的修改，不能推送，必须先拉取。
            - 拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作即可。
         2. 类比
            - 债权人：老王【项目组长】
            - 债务人：小刘【码农一号】
            - 老王媳妇：【码农二号】
            - 
            - 老王说：10天后归还，小刘接受
            - 老王媳妇说：5天后归还。小刘不能接受，老王媳妇需要找老王确认后再执行。

      8. ssh免密登录

         1. 删除已有的ssh
         2. cd ~
         3. rm -r .ssh/
         4. ssh-keygen -t rsa -C shaunlx@foxmail.com
         5. cd ~/.ssh
         6. ll
         7. cat id_rsa.pub

4. ## GitLab服务器环境搭建

5. ## Git图形化界面操作