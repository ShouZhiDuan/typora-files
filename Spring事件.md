# Spring事件

## 一、相关类

- ApplicationEvent就是Spring的事件接口
- ApplicationListener就是Spring的事件监听器接口，所有的监听器都实现该接口
- ApplicationEventPublisher是Spring的事件发布接口，ApplicationContext实现了该接口
- ApplicationEventMulticaster就是Spring事件机制中的事件广播器，默认实现SimpleApplicationEventMulticaster

## 二、事件流程

当一个事件源产生事件时，它通过事件发布器ApplicationEventPublisher发布事件，然后事件广播器ApplicationEventMulticaster会去事件注册表ApplicationContext中找到事件监听器ApplicationListnener，并且逐个执行监听器的onApplicationEvent方法，从而完成事件监听器的逻辑。

在Spring中，使用注册监听接口，除了继承ApplicationListener接口外，还可以使用注解@EventListener来监听一个事件，同时该注解还支持SpEL表达式，来触发监听的条件，比如只接受编码为001的事件，从而实现一些个性化操作。

## 三、SpringBoot常见默认事件

- ApplicationStartingEvent：springboot启动开始的时候执行的事件
- ApplicationEnvironmentPreparedEvent：spring boot对应Enviroment已经准备完毕，但此时上下文context还没有创建。在该监听中获取到ConfigurableEnvironment后可以对配置信息做操作，例如：修改默认的配置信息，增加额外的配置信息等等。
- ApplicationPreparedEvent：spring boot上下文context创建完成，但此时spring中的bean是没有完全加载完成的。在获取完上下文后，可以将上下文传递出去做一些额外的操作。值得注意的是：在该监听器中是无法获取自定义bean并进行操作的。
- ApplicationReadyEvent：springboot加载完成时候执行的事件。
- ApplicationFailedEvent：spring boot启动异常时执行事件。



## 四、事件监听器注册方式

### 1、继承ApplicationListener接口

```java
 @Component
 public class DemoListener implements ApplicationListener<DemoEvent> {
    **/****
    ** @param event* 
    ** 使用 onApplicationEvent方法对消息进行接受处理*
    **/*
    public void onApplicationEvent(DemoEvent event) {     
      String msg = event.getMsg();     
      System.out.println(msg);
    }
  
}
```

### 2、使用@EventListener方式

```java
@Configuration
@Slf4j
public class EventListenerConfig {
    
    //监听全部事件
    @EventListener
    public void handleEvent(Object event) {
        log.info("事件：{}", event);
    }
   
    //监听 CustomEvent事件
    @EventListener
    public void handleCustomEvent(DemoEvent demoEvent) {
      
    }
   
    //监听 code为oKong的事件 支持条件过滤
    @EventListener(condition="#demoEvent.msg == 'xmr'")
    public void handleCustomEventByCondition(DemoEvent demoEvent) {

    }
}
```



## 五、事件发布方式

```java
@Component
public class DemoPublisher {
   @Autowired
    ApplicationContext applicationContext;//注入 AppllcationContext用来发布事件
 
   /**
    * @param msg
    * 使用 AppllicationContext的 publishEvent方法来发布
    */
   public void publish(String msg){
      applicationContext.publishEvent(new DemoEvent(this, msg));
   }
 
}
```



```java
@RestController
@RequestMapping("/event")
@Slf4j
public class EventController {
    /**
     * 注入事件发布类
     */
    @Autowired
    ApplicationEventPublisher eventPublisher;
 
    /**
     * 参数默认注解式@RequestParam
     * @param message
     * @return
     */
    @GetMapping("/msg")
    public String push(@RequestParam("message") String message) {
        log.info("发布applicationEvent事件:{}", message);
        eventPublisher.publishEvent(new DemoEvent(this, message));
        return "事件发布成功!";
    }
 
    @GetMapping("/obj")
    public String pushObject(String message) {
        log.info("发布对象事件:{}", message);
        eventPublisher.publishEvent(message);
        return "对象事件发布成功!";
    }
}
```

> Spring中，事件源不强迫继承ApplicationEvent接口的，也就是可以直接发布任意一个对象类。但内部其实是使用PayloadApplicationEvent类进行包装了一层。这点和guava的eventBus类似。而且，使用@EventListener的condition可以实现更加精细的事件监听，condition支持SpEL表达式，可根据事件源的参数来判断是否监听。



## 六、事件异步处理

> 默认情况下，监听事件都是同步执行的。在需要异步处理时，可以在方法上加上@Async进行异步化操作。此时，可以定义一个线程池，同时开启异步功能，加入@EnableAsync。

```java

/**
 * 监听msg为xmr的事件
 */
@Async
@EventListener(condition="#demoEvent.msg == 'xmr'")
public void handleCustomEventByCondition(DemoEvent demoEvent) {
    //监听 CustomEvent事件
    log.info("监听到msg为'xmr'的DemoEvent事件，消息为：{}, 发布时间：{}", demoEvent.getMsg(), demoEvent.getTimestamp());
}
```



## 七、关于事务绑定事件

> 当一些场景下，比如在用户注册成功后，即数据库事务提交了，之后再异步发送邮件等，不然会发生数据库插入失败，但事件却发布了，也就是邮件发送成功了的情况。此时，我们可以使用@TransactionalEventListener注解或者TransactionSynchronizationManager类来解决此类问题，也就是：事务成功提交后，再发布事件。当然也可以利用返回上层(事务提交后)再发布事件的方式了，只是不够优雅而已罢了



## 八、ApplicationListener和ContextRefreshedEvent

> 很多时候我们想要在某个类加载完毕时干某件事情，但是使用了spring管理对象，我们这个类引用了其他类（可能是更复杂的关联），所以当我们去使用这个类做事情时发现包空指针错误，这是因为我们这个类有可能已经初始化完成，但是引用的其他类不一定初始化完成，所以发生了空指针错误，解决方案如下：
>
> 1、写一个类继承spring的ApplicationListener监听，并监控ContextRefreshedEvent事件（容易初始化完成事件）
>
> ApplicationListener和ContextRefreshedEvent一般都是成对出现的
>
> 在IOC的容器的启动过程，当所有的bean都已经处理完成之后，spring ioc容器会有一个发布事件的动作。从 AbstractApplicationContext 的源码中就可以看出
>
> 当ioc容器加载处理完相应的bean之后，也给我们提供了一个机会（先有InitializingBean，后有ApplicationListener<ContextRefreshedEvent>），可以去做一些自己想做的事。其实这也就是spring ioc容器给提供的一个扩展的地方。
>
> 一个最简单的方式就是，让我们的bean实现ApplicationListener接口，这样当发布事件时，[spring]的ioc容器就会以容器的实例对象作为事件源类，并从中找到事件的监听者，此时ApplicationListener接口实例中的onApplicationEvent(E event)方法就会被调用，我们的逻辑代码就会写在此处。这样我们的目的就达到了





