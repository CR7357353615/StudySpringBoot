# SpringBoot注解
---

## @SpringBootApplication
SpringBoot中首先要提到的就是@SpringBootApplication注解，这个是SpringBoot的入口注解。

源码中，SpringBootApplication注解上还有其他注解：
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

}
```
前四个注解，是元注解，没有什么实际功能，目前不需要去了解。

后三个注解，是具有具体功能的注解，需要进一步了解其作用。

##  @SpringBootConfiguration

@SpringBootConfiguration是一个配置注解，就像是xml配置文件，从源码中可以看出，这个注解主要还是使用了@Configuration的功能。由此看出，@Configuration相当于xml配置文件，这里使用Java代码可以检查类型安全

```java
@Configuration
public @interface SpringBootConfiguration {

}
```
使用@SpringBootConfiguration：
```java
@SpringBootConfiguration  
public class Config {  
  @Bean  
  public String hello(){  
      return "Hello World";  
  }  
}  
```
使用了@SpringBootConfiguration，就说明这是一个配置文件类，会被@ComponentScan扫描到。

使用@Bean注解就相当与之前在spring配置文件中声明一个< bean>。上面的bean相当于：
```xml
<bean id="hello" class="String"></bean>
```
## @EnableAutoConfiguration
@EnableAutoConfiguration是SpringBoot的核心功能，自动配置，根据当前引入的jar包进行自动配置，比如引用了jackson的jar包，就会自动配置json转换。

# @ComponentScan
@ComponentScan是自动扫描注解，默认扫描当前包和子包，和xml配置自动扫描效果一样。这里使用的@Filter是过滤掉两个系统类

## @RestController
@RestController是@Controller和@ResponseBody的合集，表示这是个REST风格的Controller，并且其方法会将返回值直接填入http响应体中。

## @Autowired
自动织入，默认按类型

## @Resource
自动织入，默认按名称

## @PathVaria，@RequestParam，@RequestBody
### @PathVariable
在URL中直接获取相应的值
```java
@RequestMapping("/pets/{petId}")  
public void findPet(@PathVariable String petId) {      
   // implementation omitted
}  
```
### @RequestParam
接收简单类型，比如String，Integer等。

也可以用来处理Content-Type为application/x-www-form-urlencoded编码的内容，提交方式可以为GET，POST

有两个属性，一个是value，一个是required。value表示传入值的名称，required表示参数是否必须传入。
```java
@RequestMapping(method = RequestMethod.GET)  
public String setup(@RequestParam(value="petId",required="false") int petId) {  

}  
```
### @RequestBody
常用来处理Content-Type不是application/x-www-form-urlencoded编码的内容。比如application/json，application/xml等。

## @RequestMapping
提供路由信息，负责URL到Controller中具体方法的映射
## @Service
一般用于修饰service层的组件
## @Repository
修饰DAO层和repositories。使用@Repository可以确保DAO或者repositories提供异常转译
## @Component
泛指组件，当组件不好归类的时候，就使用@Component。
## @Qualifier
当有多个同一类型的Bean时，可以用Qualifier("name")指定@Autowired的对象。比如IUserService有多个实现UserServiceImpl和UserServiceImpl1，都使用了@Service，在Controller层中
```java
@Autowired
private IUserService userService;
```
此时不知道该使用UserServiceImpl还是使用UserServiceImpl1。这时就应该使用@Qualifier指定使用哪个具体的实现。
```java
@Autowired
@Qualifier("userServiceImpl")
private IUserService userService;
```
