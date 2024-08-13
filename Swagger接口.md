# API技术核心原理

## REST

`RE`: representational;   `ST`: State Transfer :资源的表示 和状态的传输

- 不同资源用不同的URL表示

- 服务端通常不保存状态

## graphQL

## RPC

- 使用gRPC框架

- protocal buffers

- 流式结构

- 基于HTTP2, 不能直接从浏览器调用gRPC服务

- 常用与分布式服务器的微服务

- 可以使用基于TypeScript的tRPC框架

# Django + REST + Framework

- module 层从数据库取数据

- vue, react编写的html,css,js,会被存储到一个Nijax静态文件服务器

- 通过触发Ajax请求去请求数据

- REST是面向资源开发

## CBV类视图-Class Based View

python use the function `getattr(self, func_name)` to implement the funciton of reflection.

there are two important methods:

- as_view()
  
  - as_view()加载的时候将返回一个view函数
  
  - view函数根据用户访问的URL调用dispatch返回不同请求：get,post,put,delet

- dispatch

```python
    def dispatch(self, request, *args, **kwargs):
        # Try to dispatch to the right method; if a method doesn't exist,
        # defer to the error handler. Also defer to the error handler if the
        # request method isn't on the approved list.
        if request.method.lower() in self.http_method_names:
            handler = getattr(
                self, request.method.lower(), self.http_method_not_allowed
            )
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)
```

## Django Rest Framework

```bash
pip install djangorestframework
```

django的request.post只能使用urlencoded数据，不能解析json数据

## APIView

```python
from rest_framework.views import APIView
```

it inherits from the class `View` from the package of `Django`

it rewrite the function `dispatch` and create a new `request` object.

- 初始化：认证，权限，限流

- POST request can get the `urlencoded` format data

```python
"contentType:urlencoded \r\n\r\na=1&b=2"
# json
"contentType:json \r\n\r\{'a':1,'b':2}"
```

## 序列化器 - Serializer

### REST Interface

- `/book/` GET 查看所有资源，返回所有资源

- `/book/` POST 添加资源， 返回添加的资源

- `/book/1` GET 产看某个资源， 返回这一个资源

- `/book/1` PUT 编辑某一个资源，返回编辑之后的这个资源

- `/book/1` DELETE 删除某一个资源，返回NULL

### Serializer 校验

- 创建数据库的时候，字段是否允许为空值

- 校验规则最终要服从数据库的校验规则

- 假如数据库字段不能为null,但是校验规则`required=True`也会报错

### Serializer的save函数

调用`Serializer`的`save`函数，需要实现接口函数`create`

## View-视图

- get_serializer_class

- get_serializer

- get_queryset

- get_object

## authenticator的流程

- 调用APIView的dispatch方法中，执行self.initialize_request,构建request对象

- 这个request对象有一个user方法

- 在执行self.initial的时候，有函数perform_authentication，执行该函数调用request.user

- 一旦调用request.user,将执行self._authenticate函数在该函数内实现authenticate

- `authentication_classes=[]`

# Django

- 用户认证 - 依靠中间件

- 用户注销 - `request.session.clear()`

# Swagger规范

OpenAPI Specification : REST API的描述格式

- get: read data

- post: write data or add new data

- put: update or modify data

- delete: detelet data

- 每个操作的参数。输入和输出

- 认证方法

- 声明，团队信息

Open API may use `YAML` or 'JSON' to generate document.

# gRPC Framework

RPC: Remote Procedure Call. 允许分布在不同计算机上的应用程序能够像调用本地方法一样进行通讯。

三个必须的工具 - Python

- grpcio

- grpc-tools

- protobuf

## 传输方式

- unary 单程

- stream 双向，
