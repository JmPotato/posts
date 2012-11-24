#精简Catsup的配置文件
- tags: Catsup, Config

----

whtsky终于把反人类的`config.py`文件精简了一下，现在我的配置如下：
```python
#coding=utf-8
site_title = 'PotatoJun'
site_description = "Potato's blog"
site_url = 'http://potato-jun.tk/'
disqus_shortname = 'catsup'

#static_url = 'static'
theme_name = 'sealscript'
#google_analytics = ''

#gzip = True

twitter = 'PotatoBrothers'
github = 'PotatoBrother'

feed = 'feed.xml'
post_per_page = 5

links = (
    ('whtsky', 'http://whouz.com', 'I write catsup'),
    ('Linyi.S.Chen', 'http://linyis.tk/', "Linyi.S.Chen' blog"),
    ('Jy', 'http://jyprince.me/', "Jy's blog"),
    ('PBB', 'http://pbb.whouz.com/', "A lightweight bbs"),
    ('Catsup', 'https://github.com/whtsky/catsup', "Catsup's GithubRepositories"),
)
```