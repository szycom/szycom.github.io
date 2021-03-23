## Publishers/Subscribers

> [Redis][1]以发布者（publishers）和接收者（subscribers）解耦的方式实现了消息发布和订阅功能。
> 发布消息时，发布者只需将消息写入指定频道，而不必关心消息接收者的信息；接收消息时订阅者只需关注指定频道，并接收频道中消息，而不必关心消息发布者的信息。

## Redis连接订阅模式

客户端与Redis server建立连接，发送[SUBSCRIBE][2]命令后，建立的连接就进入了订阅状态。
**注意**
进入订阅状态后该连接就只能发送[SUBSCRIBE][2] [PSUBSCRIBE][3] [UNSUBSCRIBE][4] [PUNSUBSCRIBE][5] [PING][6] [QUIT][7]这些命令。

[1]:https://redis.io/topics/pubsub
[2]:https://redis.io/commands/subscribe
[3]:https://redis.io/commands/psubscribe
[4]:https://redis.io/commands/unsubscribe
[5]:https://redis.io/commands/punsubscribe
[6]:https://redis.io/commands/ping
[7]:https://redis.io/commands/quit
