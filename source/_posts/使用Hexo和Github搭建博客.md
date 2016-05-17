---
title: 使用Hexo和Github搭建博客
comments: true
date: 2016-05-17 11:22:13
tags:
---

根据官网[Hexo]的描述，Hexo是一个快速、简洁且高效的博客框架。
使用Hexo创建博客后，可以很方便的部署到Github上面。
下面来讲一下具体的步骤。

1. 首先安装[Node.js]和[Git]，如果已安装，可跳过此步。
2. 使用命令``` npm install -g hexo-cli ```安装Hexo。
3. 在安装完Hexo后，就可以使用它创建博客了，使用命令``` hexo init blogName ```来初始化一个博客。
4. 创建成功后，可以使用```hexo server```来查看博客的效果。

以上的步骤完成后，也只是在本地创建了一个博客，只能在本地访问，如果想要让其他人也可以看到自己的博客，那就需要部署到服务器上，幸好有[Github Pages]，直接托管自你的git仓库，也就是说，你只需要把你创建的博客放到Github里，然后只需要一些设置，就可以通过网络访问到你的博客了，是不是很方便？😄

如果你要使用Github Pages，那么你的git仓库需要这样：
1. 仓库的名字需要是你的用户名＋github.io，也就是说，假如你的Github用户名是someone，那么你的仓库名字就必须是someone.github.io。
2. 网站所在的分支必须是master。

如果二者皆满足，那么你就可以通过someone.github.io访问到你的博客。

由于使用Hexo创建的post都是.md格式的，假设我们把博客放在了master分支上，之前说到网站的分支必须是master，也就是说部署之后生成的html、css、js等文件会和原来的文件一起都在master分支上，这对我肯定是不能接受的，看着太混乱了。

那么有什么办法可以解决呢？

第一种就是，创建两个仓库，存放不同的文件；
第二种就是，同一个仓库创建不同的分支，不同的分支存放不同的文件。

下面来讲一下第二种方法，即再创建一个分支用来存放部署前的文件，即原始文件，在master上面才存放生成的文件，这样两者就不会混合在一起了。

{% codeblock %}
cd /path/to/blog                                                  // 进到blog目录
git init                                                          // 初始化git仓库
echo "# someone.github.io" >> README.md                           // 创建README文件
git add README.md .gitignore                          
git commit -m "initial commit"                                    // commit
git remote add origin git@github.com:yourName/repositoryName.git
git push -u origin master
{% endcodeblock %}

记住，master分支是用来存放生成后的网站文件，即html等。下面我们创建另外一个分支用来存放原始文件。

{% codeblock %}
git checkout -b develop
git add .
git commit -m "generate hexo blog"
git push --set-upstream origin develop
{% endcodeblock %}

我们成功的将原始文件与网站的文件分开存放在了不同分支上面。下面来讲一下如何才能将博客部署到Github Pages上面。

1. 修改_config.yml
{% codeblock %}
url: http://repositoryName
{% endcodeblock %}
2. 首先需要```npm install hexo-deployer-git --save```，然后修改_config.yml
{% codeblock %}
deploy:
  type: git
  repo: git@github.com:yourName/repositoryName
  branch: master
{% endcodeblock %}

接下来我们就可以一键部署到Github Pages上面了。
{% codeblock %}
hexo clean
hexo deploy
{% endcodeblock %}

此时你再打开```http://repositoryName```，你就可以看到你的博客了。






[Hexo]:https://hexo.io/zh-cn/
[Node.js]:https://nodejs.org
[Git]:https://git-scm.com/
[Github Pages]:https://pages.github.com/
