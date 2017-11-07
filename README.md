# Guide
https://github.com/HeinrichLi/SpringCloud-Learning
## Spring Cloud 
### Hystrix
* How to use: https://github.com/Netflix/Hystrix/wiki/How-To-Use
* Document https://github.com/Netflix/Hystrix
* 解决Feign/Ribbon第一次请求失败的问题？</br>
Hystrix默认的超时时间是1秒，如果超过这个时间尚未响应，将会进入fallback代码。而首次请求往往会比较慢（因为Spring的懒加载机制，要实例化一些类），这个响应时间可能就大于1秒了。知道原因后，我们来总结一下解决放你。解决方案有三种，以feign为例
  * hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds: 5000
该配置是让Hystrix的超时时间改为5秒
  * hystrix.command.default.execution.timeout.enabled: false
该配置，用于禁用Hystrix的超时时间
  * feign.hystrix.enabled: false
该配置，用于索性禁用feign的hystrix。该做法除非一些特殊场景，不推荐使用。
  * https://github.com/Netflix/Hystrix/tree/master/hystrix-contrib/hystrix-javanica#configuration
  * https://www.programcreek.com/java-api-examples/index.php?api=com.netflix.hystrix.exception.HystrixRuntimeException
  * http://www.jianshu.com/p/b9af028efebb
  * http://hot66hot.iteye.com/blog/2155036
* Javanica 
同步执行
要以Hystrix命令同步运行方法，您需要使用@HystrixCommand注解来注释方法：
```
public class UserService {
...
    @HystrixCommand
    public User getUserById(String id) {
        return userResource.getUserById(id);
    }
}
...
```
在上面的例子中，getUserById方法将在新的Hystrix命令中同步处理。默认情况下，**command key**的名称是命令方法名称：getUserById， 
默认**group key** 名称是被注释方法的类名称：UserService。当然，您也可以使用@HystrixCommand的属性更改它：
```
    @HystrixCommand(groupKey="UserGroup", commandKey = "GetUserByIdCommand")
    public User getUserById(String id) {
        return userResource.getUserById(id);
    }
```
设置threadPoolKey可使用@HystrixCommand#threadPoolKey()</br>
异步执行

要异步处理Hystrix命令，您应该在命令方法中返回AsyncResult的实例，如下所示：
```
    @HystrixCommand
    public Future<User> getUserByIdAsync(final String id) {
        return new AsyncResult<User>() {
            @Override
            public User invoke() {
                return userResource.getUserById(id);
            }
        };
    }
```
命令方法的返回类型应为Future，表示应该以异步方式执行命令
https://github.com/Netflix/Hystrix/wiki/How-To-Use#Asynchronous-Execution
响应式执行

要想“Reactive Execution”，您应该在命令方法中返回一个Observable的实例，如下例所示：
```
 @HystrixCommand
    public Observable<User> getUserById(final String id) {
        return Observable.create(new Observable.OnSubscribe<User>() {

                @Override
                public void call(Subscriber<? super User> observer) {
                    try {
                        if (!observer.isUnsubscribed()) {
                            observer.onNext(new User(id, name + id));
                            observer.onCompleted();
                        }
                    } catch (Exception e) {
                        observer.onError(e);
                    }
                }

            });
    }
```
命令方法的返回类型应该是Observable。
HystrixObservable接口提供了两种方法：observe（） - 与HystrixCommand#queue()或HystrixCommand#execute()行为一样，立即开始执行命令； 
toObservable() - 一旦Observable被订阅，懒惰地开始执行命令。 
为了控制这种行为，并且在两种模式之间切换,@HystrixCommand提供了名为observableExecutionMode的特定属性。 
@HystrixCommand（observableExecutionMode = EAGER）表示应该使用observe（）方法执行observable命令; 
@HystrixCommand（observableExecutionMode = LAZY）表示应该使用toObservable（）方法来执行observable命令。
注意：默认情况下使用EAGER模式
**refer to:http://blog.csdn.net/xiaojia1100/article/details/65631778**



### Zuul
https://github.com/Netflix/zuul

### Ribbon
https://github.com/Netflix/ribbon

### Turbin
https://github.com/Netflix/Turbine

### Feign
https://github.com/OpenFeign/feign
* http://www.jianshu.com/p/b9af028efebb 
* http://blog.didispace.com/cxy-wsm-zml-8/ 

### RestTemplate
* https://www.programcreek.com/java-api-examples/index.php?class=org.springframework.web.client.RestTemplate&method=postForEntity
* https://www.concretepage.com/spring/spring-mvc/spring-rest-client-resttemplate-consume-restful-web-service-example-xml-json


