# Django

## 数据库迁移文件操作

~~~
重置某个app的migrations文件
python manage.py migrate --fake app zero
删除migrations下的所有文件
python manage.py makemigrations
python manage.py migrate app --fake

查看哪些migrations文件已经执行过
python manage.py showmigrations

查看migrate对应执行的sql语句
python manage.py sqlmigrate app 0001
~~~

## 国际化信息编译

~~~
python manage.py compilemessages
~~~

## View视图关系

~~~
from django.views import View
from rest_framework.views import APIView
from rest_framework.mixins import CreateModelMixin, RetrieveModelMixin, UpdateModelMixin, DestroyModelMixin, \
    ListModelMixin
from rest_framework.viewsets import ViewSetMixin, ViewSet, GenericViewSet, ReadOnlyModelViewSet, ModelViewSet
from rest_framework.generics import GenericAPIView, ListAPIView, CreateAPIView, RetrieveAPIView, \
    DestroyAPIView, UpdateAPIView, ListCreateAPIView, RetrieveUpdateAPIView, RetrieveDestroyAPIView, \
    RetrieveUpdateDestroyAPIView

"""
django和drf视图关系

View
APIView继承View
GenericAPIView继承APIView

混入类提供用于提供基本视图行为的操作方法，不直接定义处理程序方法
CreateModelMixin
RetrieveModelMixin
UpdateModelMixin
DestroyModelMixin
ListModelMixin 
ViewSetMixin

ViewSet继承ViewSetMixin、APIView
GenericViewSet继承ViewSetMixin、GenericAPIView
ReadOnlyModelViewSet继承RetrieveModelMixin、ListModelMixin、GenericViewSet
ModelViewSet继承CreateModelMixin、RetrieveModelMixin、UpdateModelMixin、DestroyModelMixin、ListModelMixin、GenericViewSet

ListAPIView继承ListModelMixin、GenericAPIView
CreateAPIView继承CreateModelMixin、GenericAPIView
RetrieveAPIView继承RetrieveModelMixin、GenericAPIView
DestroyAPIView继承DestroyModelMixin、GenericAPIView
UpdateAPIView继承UpdateModelMixin、GenericAPIView
ListCreateAPIView继承ListModelMixin、CreateModelMixin、GenericAPIView
RetrieveUpdateAPIView继承RetrieveModelMixin、UpdateModelMixin、GenericAPIView
RetrieveDestroyAPIView继承RetrieveModelMixin、DestroyModelMixin、GenericAPIView
RetrieveUpdateDestroyAPIView继承RetrieveModelMixin、UpdateModelMixin、DestroyModelMixin、GenericAPIView
"""
~~~

