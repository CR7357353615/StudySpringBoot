# 初见SpringBoot
---
在上一家公司就接触了SpringBoot，当时由于Spring玩得还不怎么6，所以对SpringBoot还没有什么概念，只知道当时还算一个比较新的框架，网上的资源也不是很多。

今天重新接触了SpringBoot，第一感觉是配置非常简单，方便快捷。

这里不再介绍环境的搭建，可以参考以下两篇文章进行搭建。

[http://blog.csdn.net/u012702547/article/details/53740047](http://blog.csdn.net/u012702547/article/details/53740047)

[http://blog.csdn.net/Guohenghenghaha/article/details/76080229](http://blog.csdn.net/Guohenghenghaha/article/details/76080229)

## Hello World
```java
@RestController
@SpringBootApplication
public class Test19SpringBoot2Application {

  public static void main(String[] args) {
      SpringApplication.run(Test19SpringBoot2Application.class, args);
  }

  @RequestMapping(value = "/",produces = "text/plain;charset=UTF-8")
  String index(){
      return "Hello Spring Boot!";
  }
}
```

为某个类加上@SpringBootApplication代表这个类是SpringBoot项目的入口，如果再加上@RestController表示这个类是一个Controller类，可以请求。这个index就是在访问localhost:8080/时执行的方法，会返回一串字符。

## SpringBoot的配置文件
### 1.修改Tomcat默认端口和默认访问类型
```properties
server.context-path=/helloboot
server.port=8081
```
这就是将根路径设置成为helloboot，将tomcat的端口为8081
### 2.常规属性配置
```properties
book.author=罗贯中
book.name=三国演义
book.pinyin=sanguoyanyi
```
使用时直接使用@Value注入
```java
@Value(value = "${book.author}")
private String bookAuthor;
@Value("${book.name}")
private String bookName;
@Value("${book.pinyin}")
private String bookPinYin;
```

### 3.类型安全的配置
在src/main/resources文件夹下创建文件book.properties
```properties
book.name=红楼梦
book.author=曹雪芹
book.price=28
```
创建javabean
```java
@Component
@ConfigurationProperties(prefix = "book")
@PropertySource("classpath:book.properties")
public class BookBean {
    private String name;
    private String author;
    private String price;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public String getPrice() {
        return price;
    }

    public void setPrice(String price) {
        this.price = price;
    }
}
```
### 4.日志配置
```properties
logging.file=/home/sang/workspace/log.log
logging.level.org.springframework.web=debug
```

### 5.Profile配置问题
可以通过profile配置当前环境
* 1.在src/main/resources文件夹下定义不同环境下的Profile配置文件，文件名分别为application-prod.properties和application-dev.properties，这两个前者表示生产环境下的配置，后者表示开发环境下的配置
* 2.application-prod.properties中配置server.port=8081
* 3.application-dev.properties:中配置server.port=8080
* 4.然后在application.properties中进行简单配置spring.profiles.active=dev就将当前环境置为开发环境
