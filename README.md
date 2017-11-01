# log4jdbc-aop
log4jdbc的aop支持

## 需要引入maven的配置文件

```xml
<dependency>
    <groupId>com.github.loverto</groupId>
    <artifactId>log4jdbc-aop</artifactId>
    <version>1.0.0</version>
</dependency>
```


## spring的配置方法 Java Config

```java

@Configuration
@EnableAspectJAutoProxy
public class Log4JdbcConfiguretion {
    //创建Advice或Advisor
    @Bean
    public Advice dataSourceSpyInterceptor(){
        return new DataSourceSpyInterceptor();
    }
    //使用BeanNameAutoProxyCreator来创建代理
    @Bean
    public BeanNameAutoProxyCreator beanNameAutoProxyCreator(){
        BeanNameAutoProxyCreator beanNameAutoProxyCreator=new BeanNameAutoProxyCreator();
        //设置要创建代理的那些Bean的名字
        beanNameAutoProxyCreator.setBeanNames("dataSource");
        //设置拦截链名字(这些拦截器是有先后顺序的)
        beanNameAutoProxyCreator.setInterceptorNames("dataSourceSpyInterceptor");
        return beanNameAutoProxyCreator;
    }
}

```

## spring xml的配置方法

```xml
<bean id="log4jdbcInterceptor" class="net.sf.log4jdbc.DataSourceSpyInterceptor" />  
 
<bean id="dataSourceLog4jdbcAutoProxyCreator" class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">  
   <property name="interceptorNames">  
       <list>  
          <value>log4jdbcInterceptor</value>          
       </list>  
   </property>  
   <property name="beanNames">  
       <list>  
          <value>dataSource</value>  
       </list>  
   </property>  
</bean>

```
