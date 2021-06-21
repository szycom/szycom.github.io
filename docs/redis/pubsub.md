## Publishers/Subscribers

> [Redis][1]以发布者（publishers）和接收者（subscribers）解耦的方式实现了消息发布和订阅功能。
> 发布消息时，发布者只需将消息写入指定频道，而不必关心消息接收者的信息；接收消息时订阅者只需关注指定频道，并接收频道中消息，而不必关心消息发布者的信息。

## Redis连接订阅模式

客户端与Redis server建立连接，发送[SUBSCRIBE][2]命令后，建立的连接就进入了订阅状态。  
**注意**  
进入订阅状态后该连接就只能发送[SUBSCRIBE][2] [PSUBSCRIBE][3] [UNSUBSCRIBE][4] [PUNSUBSCRIBE][5] [PING][6] [QUIT][7]这些命令。

```html
尊敬的校领导：

	本人孙振亚于2021年4月10号被巨野县第二中学录用，签订了三方就业协议书。由于个人原因，不能去学校工作。因此，向学校提出解约申请，希望学校允许本人于巨野县第二中学签订的三方就业协议。本人将严格按照就业协议承担责任，并履行协定中规定的违约条款。
                                                               






 申请人：孙振亚
                                          2021年06月21日
```html

[1]:https://redis.io/topics/pubsub
[2]:https://redis.io/commands/subscribe
[3]:https://redis.io/commands/psubscribe
[4]:https://redis.io/commands/unsubscribe
[5]:https://redis.io/commands/punsubscribe
[6]:https://redis.io/commands/ping
[7]:https://redis.io/commands/quit
