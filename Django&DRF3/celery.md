## 使用Celery实现任务异步化

Celery 是一个简单、灵活且可靠的，处理大量消息的分布式系统，并且提供维护这样一个系统的必需工具。它是一个专注于实时处理的任务队列，同时也支持任务调度。

<img width="742" alt="image" src="https://github.com/JamessenLiu/python_training_202302/assets/49837274/c4f4d2bb-45e9-4bdf-94ef-b0bf891381fc">


Celery是一个本身不提供队列服务，官方推荐使用RabbitMQ或Redis来实现消息队列服务

- 创建celery实例
    
    ```python
    import os
    
    from celery import Celery
    
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "django_study.settings")
    
    app = Celery(
        'celery',
        broker="redis://@localhost:32769/4"
    )
    
    app.config_from_object(
        "apps.tasks.celery_config", silent=True
    )
    ```
    
- celery配置
    
    ```python
    from kombu import Exchange, Queue
    
    # timezone
    CELERY_TIMEZONE = 'UTC'
    
    default_exchange = Exchange('default', type='direct')
    
    CELERY_IMPORTS = ("apps.tasks.async_tasks",)
    
    CELERY_QUEUES = (
        Queue('default', default_exchange, routing_key='default', max_priority=10),
    )
    CELERYD_CONCURRENCY = 2 # celery worker number
    
    # create broker if not exists
    CELERY_CREATE_MISSING_QUEUES = True
    
    CELERYD_MAX_TASKS_PER_CHILD = 100  # max tasks number per celery worker
    
    CELERYD_FORCE_EXECV = True  # avoid deadlock
    
    CELERY_ACKS_LATE = True
    
    CELERYD_PREFETCH_MULTIPLIER = 4
    
    # speed limit
    CELERY_DISABLE_RATE_LIMITS = True
    CELERY_TASK_SERIALIZER = "pickle"
    CELERY_ACCEPT_CONTENT = ["json", "pickle"]
    
    CELERY_DEFAULT_QUEUE = 'default'
    CELERY_DEFAULT_EXCHANGE = 'default'
    CELERY_DEFAULT_ROUTING_KEY = 'default'
    ```
    

- 定义异步任务
    
    ```python
    @app.task
    def export_users():
        time.sleep(10)
        print('------------export_users------------')
        users = Users.objects.all().values()
        with open('user.csv', 'w', encoding='utf-8') as fp:
            user_file = csv.DictWriter(fp, fieldnames=['first_name', 'last_name', 'email'])
            user_file.writeheader()
            for user in users:
                user_file.writerow({
                    "first_name": user['first_name'],
                    "last_name": user['last_name'],
                    "email": user['email']
                })
        return
    ```
    
- 启动celery worker
    
    ```python
    celery -A apps.tasks.task worker -Q default --loglevel=debug
    ```
