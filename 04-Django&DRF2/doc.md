
### 跨域产生的原因

因为浏览器的同源政策，协议，端口，域名有一个不同就会造成跨域，比如说发送的异步请求是不同的两个源，就比如是不同的两个协议或者两个不同的域名或者不同的端口

### 同源政策

浏览器的机制，不允许客户端请求从A服务器请求过来的页面往B服务器发送Ajax请求。两个页面地址中的协议，域名，端口号一致，则表示同源。

### 跨域的解决方案

- 前端代理
- 浏览器插件
- 反向代理
- 后端cors配置


### DRF使用django-filter

settings配置

```python
INSTALLED_APPS = [
	...
	'django_filters'
	...
]

REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': (
        'django_filters.rest_framework.DjangoFilterBackend',
    ),
}
```

view中使用
```python
filterset_fields = ['email']
```


# 使用缓存

- 网站优化的第一定律

## 接入redis

- 安装django-redis
    
    ```python
    pip install django-redis
    ```
    
- 配置redis
    
    ```python
    CACHES = {
        "default": {
            "BACKEND": "django_redis.cache.RedisCache",
            "LOCATION": "redis://:@localhost:32769/0",
            "OPTIONS": {
                "CLIENT_CLASS": "django_redis.client.DefaultClient",
            }
        }
    }
    ```
    
- 使用声明式缓存


- 编程式缓存
    

- 缓存的更新
    - 旁路缓存模式
        
        先更新数据库，再删除缓存
        
    - 读写穿透模式
        
        先更新缓存，由缓存来更新数据库
        
    - 异步写入模式
        
        先更新缓存，异步的方式更新数据库，适用于一致性要求不高的数据。
        

### 对接BC API

```python
import requests
import json
url = 'https://api.bigcommerce.com/stores/rmz2xgu42d/v2/customers'

headers = {
    'x-Auth-Token': 'ol999cchp7xq536507sq3pbjia3fi43',
    'Content-Type': 'application/json',
    'Accept': 'application/json'
}

# 获取用户

# result = requests.get(url, headers=headers)
# customets = result.json()
# print(customets)


# 创建用户
user_data = {
    "_authentication": {},
    "company": "string",
    "email": "test123@test31.com",
    "first_name": "string",
    "last_name": "string",
    "phone": "string",
    "store_credit": 0,
    "registration_ip_address": "string",
    "customer_group_id": 0,
    "notes": "string",
    "tax_exempt_category": "string"
}

result = requests.post(url, headers=headers, data=json.dumps(user_data))
print(result)
```
