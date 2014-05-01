#在Mac上给Python安装及配置Pip

-tags:Python, Work, Pip, Mac

----

把配置过程写下来，以备不时之需。

#安装

先安装我们最需要的东西`pip`

```bash

$ wget http://python-distribute.org/distribute_setup.py
$ python distribute_setup.py

```

然后安装`pip`

```bash

$ easy_install pip

```

#配置

官方的pip源由于某种~~不可告人~~的原因访问起来很慢，咱们可以使用豆瓣的pip源，修改`~/.pip/pip.conf`文件，加入如下内容

```
[global]
index-url = http://pypi.douban.com/simple
```

如果你希望`easy_install`也能使用豆瓣源的话，修改`~/.pydistutils.cfg`

```
[easy_install]
index_url = http://pypi.douban.com/simple
```

尽情享受极速生活吧（误
