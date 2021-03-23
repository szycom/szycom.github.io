## 通过python监听过期键

```python
#-*- coding:utf-8 -*-
import time  
from redis import StrictRedis

#创建redis连接
redis = StrictRedis(host='localhost', port=6379)

#创建一个pubsub对象，该对象订阅一个频道并侦听新消息
pubsub = redis.pubsub()  
#pubsub.psubscribe('__keyspace@0__:*')

# 发布监听key失效的订阅
pubsub.psubscribe("__keyevent@0__:expired")
#通过无限循环等待事件
print('Starting message loop')  
for data in pubsub.listen():
    print(data)
```

## 参考资料

[通过python监听过期键](https://www.cnblogs.com/daysn/p/11661426.html)