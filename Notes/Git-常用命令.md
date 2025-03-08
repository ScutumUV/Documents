# Git-常用命令

一、 [配置](#配置)
	[查看git配置信息](###查看git配置信息)
	[git增加缓存区大小](###git增加缓存区大小)
	[配置用户名和密码](###配置用户名和密码)
		[查看git用户名、密码、邮箱的配置](#####查看git用户名、密码、邮箱的配置)
		[全局配置或修改](#####全局配置或修改)
		[为项目单独配置或修改](#####为项目单独配置或修改)

二、 [删除git用户名、密码、邮箱的配置](#删除git用户名、密码、邮箱的配置)

三、 [git clone 中直接配置用户名和密码](###git clone 中直接配置用户名和密码)

四、 [Git使用SSH每次提交拉取代码都需要输入密码的问题](###Git使用SSH每次提交拉取代码都需要输入密码的问题)

五、 [从远程拉取本地不存在的分支，并切换到该分支](###从远程拉取本地不存在的分支，并切换到该分支)

六、 [追加上一次提交的信息](###追加上一次提交的信息)

七、 [将工作空间中的改动追加到某次提交上](###将工作空间中的改动追加到某次提交上)

八、 [版本回退](###版本回退)

九、 [强制push](###强制push)

十、 [放弃本地修改](###放弃本地修改)

十一、 [删除分支](###删除分支)

十二、 [仓库迁移](###仓库迁移)



- ## 一、配置

    - **git config命令的–global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址**

    - ### 查看git配置信息

        ```bash
        $ git config --list
        ```

    - ### git增加缓存区大小

        ```bash
        # 将git缓存区大小设置为500MB
        git config —global  http.postBuffer 524288000
        ```

    - ### 配置用户名和密码

        - ##### 查看git用户名、密码、邮箱的配置

            ```bash
            $ git config [user.name](http://user.name/)$ git config user.password
            $ git config user.email
            ```

        - ##### 全局配置或修改

            ```bash
            # 配置authorgit config --global [user.name](http://user.name/) '用户    名'# 例：$ git config --global [user.name](http://user.name/) 'john'# 配置密码git config --global user.password '密码'# 例：$ git config --global user.password 'abc0506abc'# 配置email$ git config --global user.email '邮箱地址'
            ```

        - ##### 为项目单独配置或修改

            - cd到项目根目录中；

            - 可以直接查看 config 文件，也可以直接在命令窗口直接配置；

            - 输入以下：

              ```bash
              # 配置authorgit config user.name 'XXX'# XXX 为用户名，可以随便填（前提是你之前输入过正确的用户名和密码）# 配置密码git config user.password '123456'# 配置emailgit config user.email 'XXX'# XXX 为邮箱地址
              ```

            - 输入：

              ```bash
              git config credential.helper store
              ```





- ## 二、git clone 中直接配置用户名和密码

    ```bash
    # 用户名git clone https://username:password@remote-git-repository-url
    # 例如git clone https://roc:123456@e.coding.net/roc/example.git
    # 邮箱的方式# 如果用户名使用的是邮箱，那么 @ 符号需要转义成 %40，否则会无法识别邮箱，与 Git 仓库地址前面的 @ 造成冲突：git clone https://roc@qq.com:123456@e.coding.net/roc/example.git
    # 需要写成git clone https://roc%40qq.com:123456@e.coding.net/roc/example.git
    ```




- ## 三、Git使用SSH每次提交拉取代码都需要输入密码的问题

  ```bash
  $ ssh-keygen -p# 如果原来有密码需要先输入旧密码，# 当提示你输入新的密码的时候，按enter就可以啦，继续确认enter就可以Enter file in which the key is (/Users/never/.ssh/id_rsa):Enter old passphrase: （注：此处需要输入旧密码）
  Enter new passphrase (empty for no passphrase): （注：此处直接Enter）
  Enter same passphrase again: （注：直接Enter）
  Your identification has been saved with the new passphrase.
  ```




- ## 四、从远程拉取本地不存在的分支，并切换到该分支

  ```bash
  # 查看远端所有分支
  git branch -a
  #**从远程拉取本地不存在的分支，并切换到该分支**
  git checkout -b 本地分支名 远程分支名
  ```




- ## 五、追加上一次提交的信息

  - 修改你要修改的文件

  - add修改的文件信息

    ```bash
    git add .
    ```

  - 在上一次commit的基础上追加信息

    ```bash
  	git commit --amend
  	```
  	
  - 进入提交的vim编辑页面的编辑模式

  - 点按键盘的`i`进入编辑模式

  - 按`esc`, 输入`:wq`进行保存退出vim编辑页面

  - 如果这次提交没有push到远端，则直接push就行。如果已经push到远端了，需要使用*[Git-常用命令](Git-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%201b96f74227044bfaa1a67201f6235002.md)*

    

- ## 六、追加某一次的提交信息

  **参考：**[*https://blog.csdn.net/sky8336/article/details/108237952*](https://blog.csdn.net/sky8336/article/details/108237952)

  1. 确定需要追加的节点：`git log` ；

  2. 将HEAD移到需要更改的commit上:

        ```bash
        git rebase -i XXXX^
        # XXX为节点标识
        ```

  3. 会进入vim编辑页面；

  4. 点按键盘的`i`进入编辑模式；

  5. 找到需要更改的`commit`，将行首的`pick`改成`edit` ，按`esc` ，输入`:wq`退出（弹出的交互式界面中显示的`commit`信息，与`git log`显示的顺序相反，即父节点在上显示）；

  6. 更改文件；

  7. 使用`git add`改动的文件添加改动文件到暂存；

  8. 使用`git commit --amend`追加改动到第一步中指定的commit上；

  9. 使用`git rebase --continue`移动HEAD到最新的commit处；

  10. 解决冲突：

    1. 编辑冲突文件, 解决冲突；
    2. `git add .`；
    3. `git commit --amend`；

  11. 解决冲突之后再执行`git rebase --continue`；

  12. 最后执行push操作。

    

- ## 七、将工作空间中的改动追加到某次提交上

    **参考：**[*https://blog.csdn.net/sky8336/article/details/108237952*](https://blog.csdn.net/sky8336/article/details/108237952)]

    - **note：第1步和第3步（加粗）与上面步骤不同，其余步骤相同**

    1. 保存工作空间中的改动git stash;

    2. 将HEAD移到需要更改的commit上：

      ```bash
      git rebase XXX^ --interactive# XXX 为节点标识
      ```

    3. 执行命令git stash pop：

      ```bash
      git stash pop
      ```

    4. 使用`git add 改动的文件`添加改动文件到暂存；

    5. 使用`git commit --amend`追加改动到第一步中指定的commit上；

    6. 使用`git rebase --continue`移动HEAD到最新的commit处；

    7. 解决冲突：
        - 编辑冲突文件, 解决冲突；
        - `git add .`
        - `git commit --amend`

    8. 解决冲突之后再执行`git rebase --continue`

    9. 最后执行push操作。

        

- ## 八、版本回退

    1. 确定你要回退的版本

    ```bash
    git log
    ```

    2. 然后输入：

    ```bash
    git reset --hard HEAD~X
    # X 为回退几次。例如：git reset --hard HEAD~1
    # 回退到上一次提交，也可以写成git reset --hard HEAD^
    git reset --hard HEAD~2
    # 回退2次提交# orgit reset --hard XXX
    # XXX 为提交id
    ```

    3. 使用强制push到远端
      
       

- ## 九、强制push

    ```bash
    git push origin XXX:KKK
    # XXX 为本地分支名# KKK 为远端分支名# orgit push --force
    ```


- ## 十、放弃本地修改
  
    - **未 add 的情况：**
      
        ```bash
        # 放弃某个文件的修改git checkout -- filepathname
        # 例如：git checkout -- [readme.md](http://readme.md/)# 放弃所有文件的修 改git checkout .
        ```
        
        此命令用来放弃掉所有还没有加入到缓存区（就是 git add 命令）的修改：内容修改与整个文件删除。但是此命令不会删除掉刚新建的文件。因为刚新建的文件还没已有加入到 git 的管理系统中。所以对于git是未知的。自己手动删除就好了
        
    - **已经使用了 git add 缓存了代码：**
    
       ```bash
        # 放弃某个文件的修改git reset HEAD filepathname
        # 例如：git reset HEAD [readme.md](http://readme.md/)# 放弃所有文件的修改git reset HEAD .
       ```
       
        此命令用来清除 git 对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了如（一）所示的状态。继续用（一）中的操作，就可以放弃本地的修改。

    - **已经commit但未push：**
      
        1. 先运行 git log 查看所有的提交历史记录；
        2. 再回滚到指定的历史版本；
                
            ```bash
            git reset --hard commitid
            # commitid 是版本的id
            ```
        
            ![](Pictures/Git-常用命令.png)
    
    - **已经push了：



- ## 十一、删除分支

  - ##### 删除**本地**分支

    ```bash
    git branch -d XXX
    # XXX 为本地分支名# 以上命令如果报错，可以使用下面的命令进行强制删除git branch -D XXX
    # -D 为 --delete --force 的别名。强制删除本地分支，不考虑其合并状态。
    ```
    
  - ##### 删除**远端**分支

    ```bash
    # 首先使用下面分支查看远端所有分支git branch -agit push remote_name -d remote_branch_name
    # 例如:git push origin -d test
    ```
    

  

- ## 十二、仓库迁移

  这的迁移指的是带有之前 commit 所有节点。

  1. **随便找个文件夹，从原地址克隆一份裸版本库。如果你确定项目中所有分支的代码都是最新的，可以直接执行第二步。**

        ```bash
        git clone --bare 旧的git地址
        ```
      会在当前目录下产生一个 **xxx.git** 的文件夹，
      这个步骤，就是克隆git每一次的提交信息，
      和本地的代码没有关系，只要线上的代码是最新的，这个git版本就是完整的。

  2. **推送裸版本库到新的地址**

        ```bash
        cd xxx.git
        git push --mirror 新的git地址
        ```

  3. **删掉xxx.git文件夹**

    删不删无所谓，只是说明它没有用了而已。

  4. **代码迁移就成功了，接下来就可以使用新的地址了**

    ```bash
    git clone 新的git地址
    ```