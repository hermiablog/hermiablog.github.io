### 前情提要
>写这篇博客的目的
- 记录自己搭建过程，便于以后快速复用
- 总结经验和自己踩的坑，给其他小伙伴一些参考(由于是搭建后写的，所以没有参考图片)

>介绍
- 初步效果参考我的博客：[hermia的个人博客](https://hermiablog.com/)
- 本博客基于Hexo框架，使用github托管
- 使用自定义域名：`hermiablog.com`
- hexo主题：`hexo-theme-matery`

>主题特性
- 简单漂亮，文章内容美观易读
- Material Design 设计
- 响应式设计，博客在桌面端、平板、手机等设备上均能很好的展现
- 首页轮播文章及每天动态切换 Banner 图片(我选的图片都很美，期待你的每天访问哦)
- 瀑布流式的博客文章列表（文章无特色图片时会有漂亮的图片代替）
- 时间轴式的归档页
- 词云的标签页和雷达图的分类页
- 丰富的关于我页面（包括关于我、文章统计图、我的项目、我的技能、相册等）
- 可自定义的数据的友情链接页面
- 支持文章置顶和文章打赏
- 支持 MathJax
- TOC 目录
- 可设置复制文章内容时追加版权信息
- 可设置阅读文章时做密码验证
- Gitalk、Gitment、Valine 和 Disqus 评论模块（推荐使用 Gitalk）
- 集成了不蒜子统计、谷歌分析（Google Analytics）和文章字数统计等功能
- 支持在首页的音乐播放和视频播放功能
  - 博客正在建设中，更多功能敬请期待....

>跟着本篇文章，你会实现：
- 使用`github用户名.github.io`/自定义域名访问博客
- 设置自己喜欢的主题

>软件准备
- git
- Node.js
- 任一编辑器
- 注意：命令行操作使用git，文件内容编辑使用vscode

>参考官方文档
- [Github Pages文档](https://docs.github.com/zh/pages)
- [Hexo官方文档](https://hexo.io/zh-cn/docs/)

### 低配版：github域名+默认hexo主题
>步骤
- 新建本地仓库
- 安装hexo
- 创建github仓库
- 链接远程仓库
- 部署

#### 新建本地仓库
>我的电脑里同时使用了gitee和github仓库，且使用了不同的用户名及邮箱，因此
>- 不能使用全局的用户名、邮箱配置
>- 所以在使用SSH链接远程仓库时要单独配置

- 首先在本地新建空文件夹myblog
- 注意：**不要**使用`git init`来初始化仓库
- 因为hexo安装必须在空文件夹

#### 安装Hexo
- 右击刚刚新建的文件夹myblog，点击Git Bash Here打开git窗口
- 输入`npm install -g hexo-cli`安装Hexo
- 验证是否安装成功`hexo -v`
- 初始化Hexo：`hexo init`
- 查看是否能在本地启动成功：`hexo s`；启动服务器，访问网址之后可以看到hexo的初始界面；停止服务器：`ctrl+c`
```
npm install -g hexo-cli

hexo -v

hexo init

hexo s
```

>Hexo相关目录文件
- node_modules是node.js各种库的目录
- public是生成的网页文件目录
- scaffolds里面就三个文件，存储着新文章和新页面的初始设置
- source是我们最常用到的一个目录，里面存放着文章、各类页面、图像等文件
- themes存放着主题文件，一般也用不到。
  - 我们平时写文章只需要关注source/_posts这个文件夹就行了

#### 创建github仓库
>注意：仓库名必须是`用户名.github.io`，这有这样做，部署完之后才能使用`http://用户名.github.io`访问
- 打开`https://github.com/`，新建一个项目仓库
- 选择公开仓库和需要README文件
- git仓库默认主分支名为main，建议修改为master，和本地仓库的主分支名相同

#### 使用SSH密钥链接远程仓库
- 回到本地仓库的git界面
- 绑定用户名和邮箱
```
# 全局
git config --global user.name "yourname"
git config --global user.email "youremail"
# 局部
git config --global user.name "yourname"
git config --global user.email "youremail"
# 查看用用户名和邮箱信息是否配置成功
git config  --list
```
>- 如果你和我一样有多个仓库，就去掉 --global
>- 邮箱就是你github绑定的邮箱
>- 检查是否配置成功
- 创建SSH
```
ssh-keygen -t rsa -C 邮箱名
```
>- 后面是自己注册github的邮箱，然后敲三次回车
- 接着就会发现`C:\Users\用户名`下多了一个.ssh目录，打开后有一个公钥，一个私钥。id_rsa.pub是公钥
- 我们需要打开它，复制里面的内容
- 打开github，在头像下面点击settings，再点击SSH and GPG keys，新建一个SSH，标题随意取，把刚刚复制id_rsa.pub里面的信息粘贴到钥匙框
- 在git bash输入`ssh -T git@github.com`；如果出现`...successfully...`就成功了

#### 将hexo和GitHub关联
- 打开本地仓库，博客文件夹，在根目录找到`_config.yml`文件，使用vscode或任一编辑器打开
- 修改配置：
```
deploy:
  type: git
  repository: github地址
  branch: master
```
>- 获取repository：打开github仓库-->Code-->复制SSH地址填入即可
>- 注意：hexo的所有文件，在修改时切记**冒号后面有空格**，否则报错

#### 部署
- 安装deploy-git
```
npm install hexo-deployer-git --save
```
- 依次执行以下命令
```
# 清除缓存文件 (db.json) 和已生成的静态文件 (public)
hexo c
# 生成静态文件
hexo g
# 部署网站
hexo d
```
>- 注意：虽然我们使用的是git，但是`hexo d`会自动把文件传到github上；不需要再使用`git push`了
- 完成以上步骤，你就可以使用xxx.github.io来访问你的博客啦
- 以后写文章，只需要以下命令
```
hexo new post "文章标题"
hexo c
hexo g
hexo d
```

#### 如果你使用的是多仓库
- 如果你出现`Please tell me who you are`报错
- 如果你和我一样使用多仓库，那么：
>- 首先不要设置全局git用户名/邮箱
```
# 删除全局设置
git config --global --unset user.name
git config --global --unset user.email
```
>- 打开博客文件夹，点击`.deploy_git`文件夹-->点击窗口上的查看-->显示隐藏目录
>- 此时就会出现git的隐藏目录`.git`
>- 进去之后，打开`config`配置文件，添加以下内容，注意空格
```
[user]
email = your email 
name = your name
```
### 高配版：自定义域名+HTTPS加密协议+自定义主题
#### 自定义域名
- 首先，自定义域名需要花钱买，华为云/腾讯云/阿里云都可以
- 购买之后需要解析域名
- 打开github仓库-->点击setting-->找到pages-->拉到Custom domain处，填写你购买的域名
- 此时项目根目录会自动生成CNAME文件

#### 自定义主题
- 将心仪的主题放入根目录的`theme`文件夹中
- 根据主题文档配置

### 其他命令
#### git——多仓库配置
>!!!首先 多仓库配置 绝对不要把用户名及邮箱设置为全局
- git安装后，点击文件夹-->右击 git bash here打开
- 初始化本地仓库
```
git init
```
- 设置用户名和有效
```
git config  user.name "你的名字（一定要是英文的）"
git config  user.email "你的邮箱"

# 查看用用户名和邮箱信息是否配置成功
git config --global --list

# 删除全局设置
git config --global --unset user.name
git config --global --unset user.email
```
>添加SSH公钥
- 创建SSH密钥对
```
ssh-keygen -t rsa -C 邮箱名
```
- 如果不需要设置密码，可以直接按Enter键
- 之后就会在用户主目录下的`.ssh`文件夹中生成以下两个文件：
```
id_rsa
id_rsa.pub
```
  - 其中id_rsa为私钥，id_rsa.pub为公钥
- 因为有两个仓库，所有有两份，因此需要在`.ssh`文件夹中分开命名
  - gitee仓库的密钥：id_rsa_gitee和id._rsa_gitee.pub
  - github仓库的密钥：id_rsa_github和id._rsa_github.pub
- 将自定义路径的私钥添加到ssh秘钥搜索列表中
```
//连接认证agent（身份验证代理）
ssh-agent bash
//修改私钥路径
ssh-add ~/.ssh/id_rsa_github
```
- 将公钥内容粘贴到自己github/Gitee的设置中
  - 用记事本打开id._rsa.pub文件，复制内容
  - 登录自己的github或gitee，在个人设置中找到“安全设置”–“ssh公钥”，标题自定，将公钥粘贴进去
- 测试本机能否与github/gitee使用ssh通信
```
ssh -T git@gitee.com
//或
ssh -T git@github.com
```
  - ssh返回 “……successfully ……”，这表示可以与远程愉快的通信了 

- 本地仓库与GitHub远程仓库进行关联
```
git remote add origin 远程仓库地址(HTTP/SSH)
```

#### git基本操作
- 在本地新建README文件
```
touch README.md
```
- git add README.md命令，将刚刚新建的文件添加到暂存区中
- git status命令来检测当前git仓库的状态
- git commit -m "XXX"命令，将暂存区中的更改保存到版本库中，并对本次的更改添加注释
- git status命令来检测当前git仓库的状态
- git log，查看你的版本库

- 将本地的master分支改名为main：`git branch -m master main`；将github仓库下载到本地时，第一次提交要用`git push -u origin main`，前提是本地要有main分支

- 创建新分支：git branch 分支名
- 查看所有分支：git branch
- 切换分支：git checkout 分支名
- push到远程仓库上面：git push origin 分支名
- git branch -r 查看远程分支
- git branch -a 查看本地仓库和远程分支（a是all的简写）

>在分支上新建文件
- 切换到需要提交的分支上面 git checkout hexo
- 在hexo分支上新建文件 touch blogDesc.md
- 修改文档 vim blogDesc.md
- 编辑 i 
- 保存 esc :wq
- 提交到分支上面
  - git add 文件名/.()
  - git commit -m "描述"
  - git push -u origin hexo
- 切换到主分支上面
  - git checkout master
- 将本地分支和合并到本地主分支上
  - git merge hexo
- 远程到仓库
  - git pull origin main

>删除分支
- 查看所有的分支
  - git branch -a
- 删除远程分支
  - git push origin --delete hello
- 删除本地分支
  - git branch -D hello

>远程分支操作
- 从远程仓库中,把对应的远程分支下载到本地仓库中,保持本地分支和远程分支名称相同
```
#从远程仓库中,把对应的远程分支下载到本地仓库中,并把下载的本地分支进行重命名
git checkout -b 本地分支的名称 远程仓库名称/远程分支名称
```
- 将本地分支推送到远程仓库
```
# -u 表示把本地分支和远程分支进行关联,只在第一次推送的时候需要带-u参数
git push -u 远程仓库的名称 本地分支的名称:远程分支的名称
```
- 拉取远程分支的最新代码
```
#从远程仓库,拉取当前分支最新的代码,保持当前分支的代码和远程分支代码一致
git pull
```

#### 使用
- 新建文章 hexo new post 标题
- 部署
```
#清理之前的生成
hexo c
# 生成静态网站
hexo g
#开启本地服务 ctrl+c 停止
hexo s
#上传到github
hexo d
```
