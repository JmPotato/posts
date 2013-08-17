#PotatoBrother开发笔记——正版验证

- tags: Work, C#

----

最近用C#写了个Minecraft的启动器，基本功能都有，但是各种坑，代码也很乱，打算重构。重要的一些开发笔记，就记在博客里了。
首先说一下Minecraft的正版验证的原理，官方有一个专门验证帐号和密码的URL
```
https://login.minecraft.net/?user=帐号&password=密码&version=13
```
比如说我的账号是`ipotato@gmail.com` 密码是`XXOO`
我们就可以向Mojang的服务器发起一个post请求
```
https://login.minecraft.net/?user=ipotato@gmail.com&password=XXOO&version=13
```
于是，我们就可以得到服务器返回的一个结果，如果帐号和密码正确，会得到以下字段
```
一段数字:deprecated:游戏名称:密码的md5值
```
这时，我们就可以利用字符串的截取功能，将我们需要的用户名和密码的md5值截取下来，在启动时附加到启动参数后面，便可以实现正版启动。但如果登录失败，会返回`Bad Login`字段，这时就可以提醒用户账户或密码错误。