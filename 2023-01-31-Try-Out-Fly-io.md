# Fly.io 初体验之博客搬家

之前博客一直部署在 Vultr 每个月 $5 的日本节点上，眼看下个月就又要余额归零了，再加上一直以来整个 VM 上都只跑了 Pomash 这一个程序，算是有点浪费，所以在考虑要不要拥抱一下新时代，找一个做这种小应用部署的 SaaS，把博客程序迁移上去。目前 Pomash 在虚拟机上的搭建方式也十分老手艺：Supervisor 做进程管理，Nginx 做转发。要是能一劳永逸，干掉这些我毕生所学的建站知识，那更是再好不过了。

可能有的朋友看到这里会这样问：为啥不直接找个博客托管平台？为啥不直接用静态博客？答案也很简单，Pomash 算是我初入编程殿堂的启蒙之作，这古老的 Python Tornado Web 框架和前后端不分离的架构，以及谈得上羞耻的代码质量都保留着我那一份青春的回忆。从 14 年的第一个 Commit 算起，到今天（2023-01-31）刚好是 Pomash 的 9 周年，慢慢更新到现在，它绝对不是最好用的博客系统，但一定是我最喜欢的。

![The very first commit](https://i.imgur.com/VRN72Bp.png)

# Fly.io 是什么

[Fly.io](https://fly.io) 其实是跟同事吃饭摆龙门阵的时候了解到的一个容器化部署平台，整个产品都透露出一股小而美的气质，其提供的服务也非常简单：帮助用户用容器化的方式部署应用。人话版本就是每个人都可以~~讲 5 分钟脱口秀~~通过写一个 Dockerfile 的工作量（有些情况下甚至连 Dockerfile 都可以不用准备）快速部署可访问的应用。官方文档上所称每个账号的[免费额度](https://fly.io/docs/about/pricing/#free-allowances)如下：

- Up to 3 shared-cpu-1x 256mb VMs
- 3GB persistent volume storage (total)
- 160GB outbound data transfer

对于我这个无人问津的博客来说，使用起来应该是绰绰有余了，故而直接开整。

# flyctl

所有的部署运维操作都可以通过官方提供的命令行工具 flyctl 来完成，整个交互也极为简单，在完成 `fly auth login` 之后，即可开始部署应用了。 flyctl 的使用极为傻瓜，对于比较简单的项目，例如有 `main.go` 的 Go 项目，只需要调用 `flyctl launch`，它会扫描你的源代码结构，自动帮你生成 Dockerfile（其他语言的项目也类似），如果你只是用 Go 的标准库实现了一个简单跑在 8080 端口上的 HTTP 程序，基本上这一个命令一路 Y 过去就直接部署成功可以在浏览器里访问了。但是对于 Pomash 来说，它还需要一点额外的步骤，所以我选择自己准备一个 Dockerfile。

# 准备 Dockerfile

Pomash 是一个 Python Web 程序，运行起来很简单：`python3 run.py` 就完事了。不过不知道为什么，当年的我在实现的时候居然决定在博客跑起来前需要先手动生成 SQLite 的数据库文件，所以还得多来一步，再加上 pip 的依赖安装啥的，一点也不复杂的 Dockerfile 最后写出来长这样：

```dockerfile
# syntax=docker/dockerfile:1

FROM python:3.8-slim-buster

WORKDIR /pomash_deployment

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt


COPY . .

RUN python3 init_db.py

CMD ["python3", "run.py" , "--port=8080"]
```

接下来的操作就很简单了，`flyctl deploy` 然后根据提示输入 Y or N 就可以完成部署，我的应用名设置的是 `pomash`，所以最后部署后的地址就是 https://pomash.fly.dev。

# IPv4 地址分配

最后一步就是域名绑定了。由于 IPv4 枯竭问题，fly.io 官方选择了省着分配 IPv4 地址，只要你的应用部署时使用了默认的 80 和 443 这两个 HTTP 端口，那么就不会分配到独占的 IPv4 地址，但是每部署一个应用 fly.io 都会为你分配一个独占的公网 IPv6 地址。虽然用 CNAME 记录的方式可以把自己的域名跳转到官方给的 URL 上来解决 IPv4 的访问问题，但毕竟相比于 A 记录有一定限制，所以为了让家里还没有 IPv6 的朋友能够打开我的博客，我们可以手动分配一个独占的 IPv4 地址：`flyctl ips allocate-v4`。需要注意的是每个账户都只有一个 Dedicated IPv4 的限额，如果你想拥有 2 个及以上的公网独占 IPv4 地址的话，就只能充钱了，价格是 $2 一个月。

完成 DNS 的设置后来到网页的 Dashboard 界面，手动添加对应域名后，fly.io 会通过 Let's Encrypt 自动帮你配置免费的 SSL 证书加密。

![Dashboard](https://i.imgur.com/t4OtOYW.png)

当然，一切操作也都可以通过命令行完成，参考[官方文档](https://fly.io/docs/app-guides/custom-domains-with-fly/#configuring-certificates-before-accepting-traffic)。

# 写在最后

整个从注册到最后部署成功的过程是比较丝滑的，几乎没有遇到任何问题，官方文档也写的十分详尽，基本上我遇到的所有问题都可以在内找到详细的解决办法（例如单应用内的多进程部署），可见 fly.io 是很懂面向用户群体痛点所在的。值得一提的是，最初同事给我讲到 fly.io 倒不是因为他们的产品，而是比较有趣的招聘方式。通过他们官网的[招聘流程介绍](https://fly.io/docs/hiring/hiring/)，可以看到他们的“面试”过程很有趣，这里的面试打了引号是因为他们其实并没有面试这一步，而是通过做 2 到 3 个挑战题的方式第一阶段通关后直接加入他们的公司 Slack 和他们的工程师工作一天，一切顺利的话就会给你发 Offer。从这样一个细节来看，除去好用的产品外，这也真的是一家有趣的公司。