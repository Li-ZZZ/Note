<link rel="stylesheet" type="text/css" href="mkcss.css">

# Git学习
* ## Git设置签名  
    git需要用户设置一个**签名**  
    用户名: **username**  
    邮箱: **email**  
    ***
    注意邮箱不是真的邮箱地址 只是用来区分不同开发人员  
    这里与GitHub的账号密码没有关系  
    `git config` 是项目级别设置  
    信息保存为在项目文件.git/config   
    `git config --global` 系统全局级别设置  
    信息保存为用户根目录 ~/.gitconfig   
    ***
    `gif config --global user.name phk`  
    `gif config --global user.email phk_20@foxmail.com`

* ## Git命令
    * 本地库初始化 (先切换到工作目录下):   
    `git init `  
    * git配置  
    `git config` 设置为仓库级别  
    `git config --global` 系统全局级别设置  
    * git项目提交操作  
    `git status` 查看当前项目状态  
    `git add [filename]` 追踪文件 添加到暂存区  
    `git commit -m "commit message" [filename]` 提交文件并写提交注释  
    `git rm --cached [filename]` 把文件从暂存区中撤回  
    * 查看提交记录  
    每条记录对应一个index
    `git log` 查看提交记录 多屏显示 和linux命令操作相同  
    `git log --pretty=oneline`  
    `git log --online`  
    `git reflog `   HEAD@{移动到当前版本需要的步数}
    * 版本前进后退   
    `git reset --hard [索引值]` 基于索引前进后退  
    `git reset --hard HEAD^` 只能往后退 n个^退n个版本 
    `git reset --hard HEAD~n` 向后回退n个版本  
    reset **参数**对比  
        * --soft 仅仅移动**本地库** HEAD指针
        * --mixed 移动**本地库** 和 **暂存区**
        * --hard 移动 **本地库** 重置 **暂存区** 和 **工作区**  
    
# linux 常用命令
* `cd [dirname]` 切换目录 ~(表示用户根目录) 或 cd d:
* `ls` 展示当前目录的文件
* `pwd` 打印当前目录路径
* `cat [filename]` 查看文件 cat .gifconfig
* 多屏显示
    * 空格向下翻页
    * b向上翻页
    * q退出