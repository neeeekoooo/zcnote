事件，应用数据，监听器3者之间的关系
- 一个事件可以被多个监听器监听
- 事件触发是，传递应用数据给监听器

## 流程
- 定义事件
- 定义监听器
- 注册监听器
- 触发事件

### 定义事件
```php
class UserRegistered
{
    // 建议这里定义成 public 属性，以便监听器对该属性的直接使用，或者你提供该属性的 Getter
    public $user;

    public function __construct($user)
    {
        $this->user = $user;
    }
}
```

### 定义监听器
监听器都需要实现一下 Hyperf\Event\Contract\ListenerInterface 接口的约束方法，示例如下。
```php
class UserRegisteredListener implements ListenerInterface
{
    public function listen(): array
    {
        // 返回一个该监听器要监听的事件数组，可以同时监听多个事件
        return [
            UserRegistered::class,
        ];
    }

    /**
     * @param UserRegistered $event
     */
    public function process(object $event)
    {
        // 事件触发后该监听器要执行的代码写在这里，比如该示例下的发送用户注册成功短信等
        // 直接访问 $event 的 user 属性获得事件触发时传递的参数值
        // $event->user;
    }
}
```

### 注册监听器
在定义完监听器之后，我们需要让其能被 事件调度器(Dispatcher) 发现，可以在 config/autoload/listeners.php 配置文件 （如不存在可自行创建） 内添加该监听器即可，监听器的触发顺序根据该配置文件的配置顺序:（或者通过`@Listener`注解注册）
```php
return [
    \App\Listener\UserRegisteredListener::class,
];
```

### 触发事件
事件需要通过 事件调度器(EventDispatcher) 调度才能让 监听器(Listener) 监听到，我们通过一段代码来演示如何触发事件：
```php
<?php
namespace App\Service;

use Hyperf\Di\Annotation\Inject;
use Psr\EventDispatcher\EventDispatcherInterface;
use App\Event\UserRegistered; 

class UserService
{
    /**
     * @Inject 
     * @var EventDispatcherInterface
     */
    private $eventDispatcher;

    public function register()
    {
        // 我们假设存在 User 这个实体
        $user = new User();
        $result = $user->save();
        // 完成账号注册的逻辑
        // 这里 dispatch(object $event) 会逐个运行监听该事件的监听器
        $this->eventDispatcher->dispatch(new UserRegistered($user));
        return $result;
    }
}
```



## 总结
- 先定义事件
- 定义监听器
- 监听器可以设置监听多个事件和函数process
- 事件需要通过 事件调度器(EventDispatcher) 调度才能触发事件， 监听器(Listener) 监听后执行process



