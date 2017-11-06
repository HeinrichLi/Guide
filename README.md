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
  

### Zuul
https://github.com/Netflix/zuul

### Ribbon
https://github.com/Netflix/ribbon

### Turbin
https://github.com/Netflix/Turbine

### Feign
https://github.com/OpenFeign/feign

### RestTemplate
* https://www.programcreek.com/java-api-examples/index.php?class=org.springframework.web.client.RestTemplate&method=postForEntity
* https://www.concretepage.com/spring/spring-mvc/spring-rest-client-resttemplate-consume-restful-web-service-example-xml-json


