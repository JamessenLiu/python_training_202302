## 创建应用

- 创建Django项目
    
    ```python
    django admin startproject [name]
    ```
    
- 创建应用模块
    
    ```python
    python manage.py startapp [name]
    ```
    
- 注册应用模块
    
    ```python
    # settings.py
    INSTALLED_APPS = [
        ...
        'apps',
        'apps.modules',
        'apps.modules.companies',
        'apps.modules.users'
    ]
    ```
## 对接postgres

- 安装数据库
- 安装数据库引擎
    
    ```python
    pip install psycopg2-binary
    ```
    
- 配置数据库连接
    
    ```python
    # settings.py
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'XXX',
            'USER': 'XXX',
            'PASSWORD': 'XXX',
            'HOST': 'localhost',
            'PORT': 32768
        }
    }
    ```
    
- 执行迁移
    
    ```python
    python manage.py migrate
    ```
    
- 创建管理员账号
    
    ```python
    python manage.py createsuperuser
    ```
    
    - 配置日志
    ```python
  LOG_PATH = os.path.join(BASE_DIR, 'logs')
  if not os.path.exists(LOG_PATH):
      os.mkdir(LOG_PATH)
  LOGGING = {
      'version': 1,
      'disable_existing_loggers': False,
      'formatters': {
          'standard': {
              'format': '%(asctime)s [%(threadName)s:%(thread)d] [%(name)s:%(lineno)d] [%(levelname)s]- %(message)s',
              'datefmt': "%d/%b/%Y %H:%M:%S"
          },
      },
      'handlers': {
          'default': {
              'level': 'DEBUG',
              'class': 'logging.handlers.TimedRotatingFileHandler',
              'filename': os.path.join(BASE_DIR + '/logs/', 'all.log'),
              'when': 'D',  # this specifies the interval
              'interval': 1,  # defaults to 1, only necessary for other values
              'backupCount': 0,  # how many backup file to keep, 10 days
              'formatter': 'standard',
          },
          'console': {
              'level': 'DEBUG',
              'class': 'logging.StreamHandler',
              'formatter': 'standard'
          },
          'request_handler': {
              'level': 'ERROR',
              'class': 'logging.handlers.TimedRotatingFileHandler',
              'filename': os.path.join(BASE_DIR + '/logs/', 'traceback.log'),
              'when': 'D',  # this specifies the interval
              'interval': 1,  # defaults to 1, only necessary for other values
              'backupCount': 0,  # how many backup file to keep, 10 days
              'formatter': 'standard',
          },
          'errMsg': {
              'level': 'ERROR',
              'class': 'logging.handlers.TimedRotatingFileHandler',
              'filename': os.path.join(BASE_DIR + '/logs/', 'errLog.log'),
              'when': 'D',  # this specifies the interval
              'interval': 1,  # defaults to 1, only necessary for other values
              'backupCount': 0,  # how many backup file to keep, 10 days
              'formatter': 'standard',
          }
      },
      'loggers': {
          'django': {
              'handlers': ['default', 'console'],
              'level': 'DEBUG',
              'propagate': False
          },
          'django.request': {
              'handlers': ['request_handler'],
              'level': 'DEBUG',
              'propagate': False
          },
          'errMsg': {
              'handlers': ['errMsg', 'console'],
              'level': 'DEBUG',
              'propagate': False
          }
      }
  }
  ```
