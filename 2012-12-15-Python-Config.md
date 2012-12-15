#Windows下Python的配置

-tags:Python, EasyInstall, pip

----

#安装 Python
下载安装 Python2.7.3
地址：http://www.python.org/ftp/python/2.7.3/python-2.7.3.msi
安装目录为默认为 C:\Python27

#添加环境变量
`Win`+`R`键调出`CMD`后，输入
```bash
path = C:\Python27;C:\Python27\Scripts
```
这时候再输入`python`，如果一切正确的话，就能看到Python提示符`>>`啦
提示：按`Ctrl`+`C`来退出 Python

#安装SetupTools
打开http://pypi.python.org/pypi/setuptools/0.6c11
在最下面找到下载链接，注意对应 Python 的版本，完成后运行 exe 即可完成安装
文件名应该为`setuptools-0.6c11.win32-py2.7.exe`

#安装pip
在cmd里输入
``bash
easy_install pip
```
pip安装好后，输入`pip install`+安装包名称。
程序就会自动下载相应的安装包，并进行安装
------------------------------------------------
Windows下搞开发真的很反人类，准备转战Linux