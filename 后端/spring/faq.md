###ContentNegotiatingViewResolver vs. HttpMessageConverter+ResponseBody Annotation###
when you annotate a method @ResponseBody spring will delegate the rendering of the view to a HttpMessageConverter and not a ViewResolver. 
@ResponseBody annotate  will bypass the Viewresolver!


###HandlerMapping  Handleradapter###
Spring mvc 使用HandlerMapping来找到并保存url请求和处理函数间的mapping关系。 
  
以DefaultAnnotationHandlerMapping为例来具体看HandlerMapping的作用 
  DefaultAnnotationHandlerMapping将扫描当前所有已经注册的spring beans中的@requestmapping标注以找出url 和 handler method处理函数的关系并予以关联。 

Handleradapter 
Spring MVC通过HandlerAdapter来实际调用处理函数。 
以AnnotationMethodHandlerAdapter为例 
DispatcherServlet中根据handlermapping找到对应的handler method后，首先检查当前工程中注册的所有可用的handlerAdapter,根据handlerAdapter中的supports方法找到可以使用的handlerAdapter。通过调用handlerAdapter中的handle方法来处理及准备handler method中的参数及annotation(这就是spring mvc如何将reqeust中的参数变成handle method中的输入参数的地方)，最终调用实际的handle method。 


###Using AspectJ to dependency-inject domain objects with Spring###
http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#aop-atconfigurable


###spring 事务 失效###
```
<context:component-scan base-package="com.yunforge" use-default-filters="false">
    <context:include-filter expression="org.springframework.stereotype.Controller" type="annotation" />
</context:component-scan>
```
use-default-filters默认值就是true
它的作用就是扫描一些相关的注解，包括了@controller,@Component,@Service,@Repository等，

而use-default-filters="false",同时使用白名单context:include-filter对指定的注解进行扫描
如：
```
 <context:component-scan base-package="org.bdp.system.test.controller" use-default-filters="false">     
     <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>     
</context:component-scan>   
```
这就表示只扫描"org.bdp.system.test.controller"下的Controller注解

事物失效，应该是applicationContext.xml中已经扫描过@Service和@Repository,而在xx-servlet.xml扫描@Controller时就不应该再去扫描@Service和@Repository，不然就会导致事物失效的情况
