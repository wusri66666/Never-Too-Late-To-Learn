---
nameko使用案例
---

---

##### #### nameko简单了解:

* Nameko是一个用python语言写的微服务框架，

* 支持通过 rabbitmq 消息队列传递的 rpc 调用，也支持 http 调用。

* 小巧简洁，简单且强大；

* 可以让你专注于应用逻辑

#### nameko用途:

*  可通过RabbitMq消息组件来实现RPC服务 

#### 依赖库:

* pip install nameko

#### 小试牛刀:

```python
# server.py
# 启动方式：nameko run hello_service --broker amqp://user:password@ip
from nameko.standalone.rpc import ClusterRpcProxy

CONFIG = {'AMQP_URI': "amqp://guest:guest@localhost"}


def publish():
    with ClusterRpcProxy(CONFIG) as rpc:
        rpc.hello_service.hello()
```

```python
# nameko_service.py  启动一个服务
from nameko.rpc import rpc

class hello_service:
    name = "hello_service"   # name为自定义的服务名称

    @rpc  # 入口标记点
    def hello(self): 
        print("hello world")
```

#### Flask中使用nameko案例:

```python
from flask import Flask, request, Response
from nameko.standalone.rpc import ClusterRpcProxy

app = Flask(__name__)
CONFIG = {'AMQP_URI': "amqp://guest:guest@localhost"}


@app.route('/compute', methods=['POST'])
def compute():
    operation = request.json.get('operation')
    value = request.json.get('value')
    other = request.json.get('other')
    email = request.json.get('email')
    msg = "Please wait the calculation, you'll receive an email with results"
    subject = "API Notification"
    with ClusterRpcProxy(CONFIG) as rpc:
        # 同时调用两个服务  一个同步远程调用和一个异步远程调用
        rpc.server_publish.hello(email, subject, msg)
        rpc.server_publish01.hello.call_async(operation, value, other, email)
        return Response({"code": 1})

if __name__ == '__main__':
    app.run(debug=False)

解释一下:
    rpc后紧跟的是微服务定义时的类变量 name 的值即为微服务名称，接着紧跟rpc方法，使用 	         call_async 为异步调用，而调用 result_async.result() 时会等待异步任务返回结果。需要注     意的是， 运行 ClusterRpcProxy(config) 时会创建与队列的连接，该操作比较耗时，如果有大量     的微服务调用，不应该重复创建连接，应在语句块内完成所有调用。异步调用的结果只能在语句块内获     取，即调用 .result() 等待结果。语句块之外连接断开就无法获取了。
```

#### 方法2: 使用flask_nameko包装器

```python
# create  __init__.py
# 安装nameko模块  pip install flask_nameko
# 将nameko注册在flask_app当中, 目的就是和项目一体化, 另一方面就是可以缓解对队列的消耗(节省资源)
from flask import Flask

from flask_nameko import FlaskPooledClusterRpcProxy

rpc = FlaskPooledClusterRpcProxy()

app = Flask(__name__)
app.config.update(dict(
    NAMEKO_AMQP_URI='amqp://guest:guest@localhost'
))

rpc.init_app(app)
```

```python
from flask import Response, request

from __init__ import (
    app,
    rpc
)


@app.route('/', methods=['POST'])
def index():
    rpc.hello_service.hello.call_async(request.json)
    return Response({"code": 1, "msg": 'nihao'})


@app.route('/one', methods=['POST'])
def index01():
    rpc.hello_service01.hello01.call_async(request.json)
    return Response({"code": 1, "msg": 'nihao'})


if __name__ == '__main__':
    app.run(debug=True)
```

#### Django中使用nameko框架:

```python
1. 创建一个Django项目
2. 创建一个app
3. 安装django-nameko对应模块 pip install django-nameko
4. 在django配置文件settings中配置nameko相关信息
	# 远程amqp地址
	NAMEKO_CONFIG = {
        'AMQP_URI': 'amqp://guest:guest@localhost'
    }
    NAMEKO_POOL_SIZE = 4
    # 设置连接超时
    NAMEKO_TIMEOUT = 10
```

```python
# nameko_tools.py
from django_nameko import get_pool

def publish_msg(data):
    try:
        with get_pool().next() as rpc:
            res = rpc.hello_service.hello.call_async(data)
            return 0, res.result()
    except Exception as e:
        print(e)
        return 1, e
```

```python
# apis.py
from rest_framework.response import Response
from rest_framework.views import APIView
from nameko_tools import publish_msg


class NameKoAPI(APIView):
    def post(self, request):
        code, res = publish_msg(self.request.data)
        if code == 0:
            return Response({'code': 0, "msg": "%s"% res})
        return Response({'code': -1, "msg": "%s"% res})
```

##### 以上均为消息发布在Flask和Django中的使用案例, 使用当中中需单独运行一个nameko服务用于订阅消息:

```python
# nameko_server.py
from nameko.rpc import rpc


class Hello_service:
    name = "hello_service"  # 服务名称

    @rpc  # 标记点入口
    def hello(self, data):
        print(data)
        return "Hello, {}!".format(data)


class Hello_service01:
    name = "hello_service01"

    @rpc
    def hello01(self, data):
        print(data)
        return "Hello, {}!".format(data)
```

