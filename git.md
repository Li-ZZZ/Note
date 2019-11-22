<link rel="stylesheet" type="text/css" href="mkcss.css">

# Git学习
* ## Git设置签名  
    git需要用户设置一个**签名**  
    > 用户名: **username**  
    > 邮箱: **email**  
    
    注意邮箱不是真的邮箱地址 只是用来区分不同开发人员  
    这里与GitHub的账号密码没有关系  

    ---
    **项目级别**设置：
    >  `git config`  

    系统**全局**级别设置：
    > `git config --global`

    项目级别信息保存为在**项目文件**.git/config   
    全集级别信息保存为**用户根目录** ~/.gitconfig   
    PS：
    > `gif config --global user.name phk`  
    > `gif config --global user.email phk_20@foxmail.com`
* ## Git命令
    * git**签名**配置  
        设置为仓库级别  
        >`git config`  

        系统全局级别设置  
        > `git config --global` 
    ---
    * 本地库初始化 (先切换到工作目录下):   
        > `git init `  
    * git项目提交操作  
    `git status` 查看当前项目**状态**  
    `git add [filename]` **追踪**文件 添加到**暂存区**  
    `git commit -m "commit message" [filename]` **提交**文件并写提交**注释**  
    `git rm --cached [filename]` 把文件从暂存区中**撤回**  
    ---
    * 查看提交记录  
    每条记录对应一个**index**  
    `git log` 查看提交**详细**记录 多屏显示 和linux命令操作相同  
    `git log --pretty=oneline` 查看**单行**提交记录  
    `git log --online` 查看**单行**提交记录  
    `git reflog `   HEAD@{移动到当前版本需要的**步数**}
    * 版本前进后退   
    `git reset --hard [索引值]` 基于**索引**前进后退  
    `git reset --hard HEAD^` 只能往后退 n个^退n个版本  
    `git reset --hard HEAD~n` 向后回退n个版本  
    reset **参数**对比  
        * --soft 仅仅移动**本地库** HEAD指针
        * --mixed 移动**本地库** 和 **暂存区**
        * --hard 移动 **本地库** 重置 **暂存区** 和 **工作区**  
    * 版本差异  
    `git diff [filename]` **工作区**与**暂存区**对比  
    `git diff [本地库历史版本] [filename]` **工作区**与**本地库**对比 不带文件名 比较多个文件
    ---
    * 分支操作  
    `git branch -v` **查看**所有分支  
    `git branch [name]` **创建**分支  
    `git checkout [name]` **切换**分支  
    `git merge [name]` **合并**分支 先切换到master分支(被合并分支)  
    合并**冲突**  
    提示冲突进入 master|merging 状态  
    本地解决冲突  
    <<<<< **HEAD**  本分支修改的内容  
    =======  
    \>>>>>>  **其他分支**修改的内容  
    自己编辑完文件**删除特殊符号**后保存退出 再add commit(不能带文件名)
    ---
    * 远程库 GitHub  
    `git remote -v` **查看**远程库信息 (fetch 用来取回)(push 用来推送)  
    `git remote add origin URL` 用origin代替URL  
    `git clone URL` 从URL上**克隆**远程库    
    `git fetch` 从远程库**下载** 但没有和本地库合并  
    `git merge` **合并**本地库操作  
    `git pull` fetch+merge操作  
    如果不是基于**远程库**的最新版所做修改  
    不能**push** 要先**pull**最新版  
    本地解决**冲突**再`commit push` 和本地冲突处理相同
    ---
    * fork  
    远程库上可以fork**别人**的项目 **自己**做修改  
    `Pull requests` 请求源项目负责人将自己的修改**合并**到源项目中  
    源负责人**审核**后 `merge pull requests` 同意
    
 
# linux 常用命令
* `cd [dirname]` 切换目录 ~(表示用户根目录) 或 cd d:
* `ls` 展示当前目录的文件
* `pwd` 打印当前目录路径
* `cat [filename]` 查看文件 cat .gifconfig
* 多屏显示
    * 空格向下翻页
    * b向上翻页
    * q退出
* `ctrl+L` 清屏 

# 完结 11.13