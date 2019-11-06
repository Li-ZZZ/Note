<font size="3px">

# Git学习
* # Git设置签名  
    git需要用户设置一个**签名**  
    用户名: username  
    邮箱: email  
    注意邮箱不是真的邮箱地址 只是用来区分不同开发人员  
    这里与GitHub的账号密码没有关系  
    git config 是项目级别设置  
    信息保存为在项目文件.git/config   
    git config --global 系统全局级别设置  
    信息保存为用户根目录(~)/.gitconfig   
    gif config --global user.name phk  
    gif config --global user.email phk_20@foxmail.com

* # Git命令
    * 本地库初始化(先切换到工作目录下):   
    `git init `  
    * git配置  
    `git config` 设置为仓库级别  
    `git config --global` 系统全局级别设置  
    * git项目提交操作  
    `git status` 查看当前项目状态
# linux 常用命令
* cd 切换目录 cd ~(表示用户根目录)  或 cd d:
* ls 展示当前目录的文件
* pwd 打印当前目录路径
* cat 查看文件 cat .gifconfig