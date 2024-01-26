- [git 配置常用命令别名](#git-配置常用命令别名)
- [配置文件](#配置文件)


### git 配置常用命令别名    
```sh
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
### 配置文件
配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。   
每个仓库的Git配置文件都放在`.git/config`文件中;     
用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中。

### 将git使用代理设置为别名     
编辑`.gitconfig`文件，将下面内容加入到`[alias]`下： 
```
[alias]
    set-proxy = config --global http.proxy http://10.27.54.221:12333
    unset-proxy = config --global --unset http.proxy
```