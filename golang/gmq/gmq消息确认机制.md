# 消息确认机制
> 和大多数的消息中间件一样,确认机制采用消费者发送`ack`给`gmq`,以确认消息被消费完毕

## 流程图
![](../../images/gmq/消息确认机制.png)

## 确认模式
- 主动确认,通过设置`topic.isAutoAck`为`true`,当消费者消费完消息时,不需要`ack`确认,gmq自动移除消息
- 被动确认,即消费者需要在消费完毕时,通过`ack`告诉gmq可以移除消息

## 注意
默认情况下,当超过30秒,gmq没有得到消费者的`ack`确认,则会再次添加队列的尾部,然后等待再次被消费,并且消费次数`job.ConsumeNum`会加1,消费者消费消息时,这个次数也会返回给消费者

## 链接
- https://github.com/wuzhc/gmq