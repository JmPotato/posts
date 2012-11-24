#Catsup配置全过程
- tags: Catsup, Nginx, Blog

----

在这写一下，以免以后在配置的时候走弯路。
#配置Catsup
首先要从Github上把Catsup克隆下来，在SSH里进行如下操作
```
git clone git clone git://github.com/whtsky/catsup.git
cd catsup
pip install -r requirements.txt
cp config-sample.py config.py
```
然后，编辑config.py
我的配置如下
```python
#coding=utf-8
import os
site_title = 'PotatoJun'
site_description = "Potato's Blog"
site_url = 'http://potato-jun.tk'
static_url = 'static'
theme_name = 'sealscript'
google_analytics = ''  # optional

catsup_path = os.path.dirname(__file__)
posts_path = os.path.join(catsup_path, '_posts')
theme_path = os.path.join(catsup_path, 'themes', theme_name)
common_template_path = os.path.join(catsup_path, 'template')
deploy_path = os.path.join(catsup_path, 'deploy')

twitter = 'PotatoBrothers'
github = 'PotatoBrother'
disqus_shortname = 'potato'
foot_dcrp = 'I hope you have fun browsing my blog'
about_me = 'Potato,13 years old,Computher Fans,Cube Fans,Apple Fans'
feed = 'feed.xml'
post_per_page = 5

links = (
    ('whtsky', 'http://whouz.com', 'I write catsup'),
    ('catsup', 'https://github.com/whtsky/catsup', 'the source of this blog'),
    ('Scxy', 'http://linyis.tk/', "Linyi.S.Chen's Blog"),
    ('JyBox', 'http://jyprince.me/', "Jy's Blog"),
    ('JyBox BBS', 'http://jybox.net/', "JyBox BBS"),
)

if site_url.endswith('/'):
    site_url = site_url[:-1]
if static_url.endswith('/'):
    static_url = static_url[:-1]

settings = dict(static_path=os.path.join(theme_path, 'static'),
    template_path=os.path.join(theme_path, 'template'),
    gzip=True,
    site_title=site_title,
    site_description=site_description,
    site_url=site_url,
    twitter=twitter,
    github=github,
    feed=feed,
    post_per_page=post_per_page,
    disqus_shortname=disqus_shortname,
    links=links,
    static_url=static_url,
    google_analytics=google_analytics,
     about_me=about_me,
     foot_dcrp=foot_dcrp,
)
```
接着，把Github上的post项目克隆到catsup目录
```
git clone git://github.com/PotatoBrother/_posts.git
```
OK，配置完毕，接下来我们要把它切换成静态的，在SSH里输入：
```
python catsup.py deploy
```
Catsup配置完毕，接下来是Nginx和Webhook的配置
#Webhhok的配置
为了使Catsup可以和Github的post项目同步发表博文，我们需要Webhook，在SSH里输入：
```
python catsup.py webhook --port=1234
```
然后更改post项目的WebhookURL
#Nginx的配置
为了使http://potato-jun.tk/ 可以访问catsup
添加配置文件，内容如下：
```nginx
server { 
listen 80;
server_name potato-jun.tk; 
index index.html index.htm index.php; 
root /root/catsup/deploy/; 
access_log off;
}
```
好了，一切完毕，你的Catsup就可以跑起来了！
