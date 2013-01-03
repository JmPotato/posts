#Markdown转义

- tags:Markdown,HTML,tornado,Python

----

想学习下Tornado和HTML，便写了个这个~~蛋疼的~~小程序。
首先是HTML，不得不说HTML真的很好学习。这个程序只有一个很简单的文本框和一个按钮
```html
<!DOCTYPE html>
<html>
    <head>
    	<meta charset="utf-8">
    	<title>
    		MarkdownEscape
    	</title>
        <style>
        body{
            background-color: #EEE;
        }
        textarea{
            width: 100%;
            }        
        form{
            margin-top: 10px;
        }
        </style>
    </head>

    <body>
    	<form action="" method="post">
    		<textarea rows=30 name="content">
This is a MarkdownEscape——The Markdown is converted to HTML</textarea>
    		<button type="submit" class="btn btn-primary">Ok</button>
    	</form>
    </body>
</html>
```
接下来就是Python部分，很简单，只有21行
```python
import markdown
import tornado.httpserver
import tornado.web
import tornado.ioloop

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.render("page.html")

    def post(self):
        content = self.get_argument("content")
        html = markdown.markdown(content,['codehilite', 'fenced_code'])
        self.write(html)

if __name__ == '__main__':
    application = tornado.web.Application([
            (r'/', MainHandler),
        ])
    http_server = tornado.httpserver.HTTPServer(application)
    http_server.listen(8888)
    tornado.ioloop.IOLoop.instance().start()
```
功能很简陋，没有代码高亮什么的，等我先研究研究再添加吧