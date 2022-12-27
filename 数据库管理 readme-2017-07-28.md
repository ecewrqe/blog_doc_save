## 作者信息：
* 作者：李啸宇
* 日期：2016-10-31
* email：euewrqe@gmail.com
* GitHub：[https://github.com/euewrqe/]
* python: 3.5.0
* django: 1.11.0
* version: 1.0

## admin
模拟django的admin的数据库管理系统，可插入任何项目。

**主要目录**
```
myadmin:
  - baseadmin.py
  - baseform.py
  - urls.py
  - views.py
template:
  - myadmin:
    - admin_layout.html
    - admin_index.html
    - admin_index_app.html
    - admin_table_list.html
    - admin_table_add.html
    - admin_table_change.html
    - component:
      - table_add_or_change_form.html
static:
  - css:
    - vendor:
      - bootstrap(d):
	    - bootstrap.min.css
	  - fontawesome(d):
  - js:
    - jquery.js
    - vendor:
      - bootstrap(d):
      - cookie:
        - jquery.cookie.js
utils:
  - tools.py

----- url配置

urlpatterns = [
    url(r'^(\w+)/(\w+)/add/$', views.table_add),
    url(r'^(\w+)/(\w+)/(\d+)/change/$', views.table_change),
    url(r'^(\w+)/(\w+)/$', views.table_list),
    url(r'^(\w+)/$', views.app_model),
    url(r'^get-action/$', views.get_action),
    url(r'^batch-update/$', views.batch_update),
    url(r'^$', views.admin),
]
```

假设有这些表：
```
from django.db import models

# Create your models here.
class Blog(models.Model):
    title=models.CharField(max_length=128)
    content=models.TextField()
    account=models.ForeignKey('Account')
    blogterm=models.ForeignKey('BlogTerm')
    tag=models.ManyToManyField('Tag')


class BlogTerm(models.Model):
    name=models.CharField(max_length=128)


class Account(models.Model):
    username=models.CharField(max_length=128)
    password=models.CharField(max_length=128)

    class Meta:
        verbose_name="用户名"

class Tag(models.Model):
    name=models.CharField(max_length=128)
```

导入admin到项目中，创建app_admin.py和app_form.py
app.app_admin.py
``` python
baseadmin.site.register(models.Account)
baseadmin.site.register(models.Blog)
baseadmin.site.register(models.BlogTerm)
```

admin.views.py
``` python
#导入settings
from admin_pro import settings

# 导入app_admin
for app in settings.INSTALLED_APPS:
    try:
        __import__("%s.app_admin"%app)
    except ImportError:
        pass
```
admin可自行定制
```
#继承baseadmin.create_admin()返回值

baseadmin = type('BaseAdmin', (object,), {
		# 这些属性可自行定制
        "list_display": (),  # 列表中指定列出项
        "list_filter": (),  # 筛选项
        "search_fields": (),
        "model_change_form": None,
        "model_add_form": None,
        "filter_horizontal": (),  # 多对多特殊样式
        "actions": (),
        "list_editable": (),
    })

class BlogAdmin(baseadmin.create_admin()):
    list_display=("title",'account','blogterm','date',)


```











