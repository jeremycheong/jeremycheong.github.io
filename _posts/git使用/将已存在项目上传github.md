### 前言
在github上新建项目就不说了，但是在新建项目的时候，如果不选择生成readme和license的话会出现操作步骤。          
但是如果添加了readme和license文件，那么就相当于项目已经上传一次了。这时候如何将已存在的项目上传。

### 已存在的项目设置git配置
* 初始化git

```sh
git init
```

* 创建并编辑`.gitignore`
* 添加，提交并设置远程仓库地址

```sh
git add . -v

git commit -m "updata"

git remote add origin https://github.com/jeremycheong/kcarhome.git 
```

* 先拉去远程仓库代码（因为在创建时添加了readme和license文件）          

```sh
git pull origin master  --allow-unrelated-histories
```

* 将现有项目推上去

```sh
git push -u origin master
```





