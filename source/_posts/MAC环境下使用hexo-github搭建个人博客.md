---
title: MAC环境下使用hexo+github搭建个人博客
date: 2017-12-19 11:22:36
tags:
---

环境检查 :
```
node -v
npm -v
git --version
```
1.
```
npm install -g hexo
hexo -v
```
2. 新建个hexo文件夹, cd到文件目录下
3. > hexo init
4. > npm install hexo --save
5. > hexo g (或 hexo generate)
6. > hexo s (或 hexo server)
7. 这个时候应该会提示 :
```
INFO Start processing
INFO Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
这个时候就可以通过访问 [http://localhost:4000/](http://localhost:4000/)进行预览了<br/>
注意: 如果在hexo s操作报错的话, 可以运行下面命令在重新尝试
> npm install hexo-server --save
8. 安装主题
```
可以在github上搜索hexo theme, 其中有很多主题, 以下以next主题为例
在本地hexo目录下输入:
git clone https://github.com/iissnan/hexo-theme-next themes/next
安装完成后找到themes文件夹, 会发现里面多了个next文件夹
接下来回到hexo根目录下打开_config.yml文件
修改 theme: landscape  =>  theme: next
在运行 hexo s  打开http://localhost:4000/
```
9. 修改blog页面
```
打开根目录下的_config.yml
# Site
 title: 网站标题
 subtitle: 副标题
 description: 个人签名
 author: 姓名
 language: zh-Hans
 timezone: Asia-Shanghai
```
10. 在github上新建仓库,在Repository name下面填写要创建的地址,这个地址是域名,比如:camellieeee.github.io
11. 部署到hexo. 打开hexo目录
> vim _config.yml
> 寻找deploy属性(可用命令/deploy  n=>下一个  N=>上一个)
找到后修改为以下内容
```
deploy:
    type: git
    repository: https://github.com/Camellieeee/Camellieeee.github.io
    branch: master
```
repository对应的是github仓库地址,注意冒号后面的空格
12. 运行以下命令进行部署
```
npm install hexo-deployer-git --save
git init
git remote add origin https://github.com/camellieeee/camellieeee.github.io
hexo clean
hexo g
hexo d (或 hexo deploy)
```
这样就能在[https://username.github.io](https://username.github.io)上看到自己的博客了
13. 发布新文章:
```
hexo new post '文章标题'
前往hexo/source/_posts文件夹,找到新建的文章进行编辑
编辑完成后更新到github
hexo clean
hexo g
hexo d
```
14.[hexo API: https://hexo.io/zh-cn/docs/writing.html](https://hexo.io/zh-cn/docs/writing.html)
