# Celery

## What is Celery?

### Task queue

- A task queue is a queue for tasks. It cooperates with multiple works to allocate the tasks to them.

Some common task queues (or called brokers) is RabbitMQ and Redis broker.

## Celery with Django

Common Django project layout:
```
proj/
    manage.py
    proj/
        __init__.py
        settings.py
        urls.py
```

The recommended way is to create a `proj/proj/celery.py` module that defines the Celery instance.

Then import this app in `proj/proj/__init__.py`.


# Celery Beat

