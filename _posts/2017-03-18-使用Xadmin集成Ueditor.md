---
layout: post
title: 使用Xadmin集成Ueditor
date: 2017-03-18
tag: 博客
---

## 一 安装

* pip命令安装，由于ueditor为百度开发的一款富文本编辑框，现已停止维护，如果解释器为python2，则直接

  ```
  pip install djangoueditor
  ```

* 解压包安装，python3以上的版本需要下载压缩包解压安装 下载地址:https://github.com/twz915/DjangoUeditor3
  步骤:

  ​	1.到下载地址下载并解压

  ​	2.cmd进入该文件夹根目录python setup.py install直接安装到当前site-package中



## 二 添加app

将djangoueditor添加到setting.py中

```
INSTALLED_APPS = [
...
'DjangoUeditor',]
```



## 三 添加url

将URL添加到urlpatterns中去：

```

#富文本编辑器
url(r'^ueditor/',include('DjangoUeditor.urls' )),
```



## 四 修改models字段

django自带的富文本编辑框为models.Textfield()

```
 from DjangoUeditor.models import UEditorField

    class Course(models.Model):
        name = models.CharField(max_length=20, verbose_name='课程名称')
        desc = models.TextField(verbose_name='课程描述')
        detail = UEditorField(verbose_name='课程详情',width=600, height=300, toolbars="full", imagePath="course/ueditor/", filePath="course/ueditor/", upload_settings={"imageMaxSize":1204000},default='')
```

说明：
width，height :编辑器的宽度和高度，以像素为单位。imagePath :图片上传后保存的路径,如"images/",实现上传到"{{MEDIA_ROOT}}/images"文件夹。 注意：如果imagePath值只设置文件夹，则未尾要有"/" imagePath可以按python字符串格式化：如"images/%(basename)s_%(datetime)s.%(extname)s"。这样如果上传test.png，则文件会 被保存为"{{MEDIA_ROOT}}/images/test_20140625122399.png"。 filePath : 附件上传后保存的路径，设置规则与imagePath一样。

## 五 Xadmin中添加插件Ueidtor

由于已经将xadmin源文件拷贝到了项目下，本文为extra_apps/xadmin，在xadmin下的plugin中新建一个ueditor.py文件，里面写入如下：

```
import xadmin
from xadmin.views import BaseAdminPlugin, CreateAdminView, ModelFormAdminView, UpdateAdminView
from DjangoUeditor.models import UEditorField
from DjangoUeditor.widgets import UEditorWidget
from django.conf import settings


class XadminUEditorWidget(UEditorWidget):
    def __init__(self,**kwargs):
        self.ueditor_options=kwargs
        self.Media.js = None
        super(XadminUEditorWidget,self).__init__(kwargs)


class UeditorPlugin(BaseAdminPlugin):

    def get_field_style(self, attrs, db_field, style, **kwargs):
        if style == 'ueditor':
            if isinstance(db_field, UEditorField):
                widget = db_field.formfield().widget
                param = {}
                param.update(widget.ueditor_settings)
                param.update(widget.attrs)
                return {'widget': XadminUEditorWidget(**param)}
        return attrs

    def block_extrahead(self, context, nodes):
        js = '<script type="text/javascript" src="%s"></script>' % (settings.STATIC_URL + "ueditor/ueditor.config.js")         #自己的静态目录
        js += '<script type="text/javascript" src="%s"></script>' % (settings.STATIC_URL + "ueditor/ueditor.all.min.js")   #自己的静态目录
        nodes.append(js)

xadmin.site.register_plugin(UeditorPlugin, UpdateAdminView)
xadmin.site.register_plugin(UeditorPlugin, CreateAdminView)
```



## 六 将ueditor添加到plugin下的_init_中

```
PLUGINS = (
...
'ueditor',
)
```



## 七 将ueditor添加到adminx.py中

```
class CourseAdmin(object):
...
style_fields = {"detail": "ueditor"}
```



## 八 前端页面转义

浏览器为了web安全在对后端传来的html代码会进行转义，会将<>等符号进行转义，因此要对页面设置过滤器防止字符转义

```

{% autoescape off %}
{{ course.detail }}
{% endautoescape %}
```



