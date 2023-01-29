---
title: 为什么 Spring 和 IDEA 都不推荐使用 @Autowired 注解？
shortTitle: 为什么 Spring 和 IDEA 都不推荐使用 @Autowired 注解？
author: 磊哥
category:
  - 微信公众号
head:
---

> 大家在使用IDEA开发的时候有没有注意到过一个提示，在字段上使用Spring的依赖注入注解`@Autowired`后会出现警告，但是使用`@Resource`却不会出现，今天我们来聊聊这两者的区别。

@Autowired 和 @Resource 都是 Spring/Spring Boot 项目中，用来进行依赖注入的注解。它们都提供了将依赖对象注入到当前对象的功能，但二者却有众多不同，并且这也是常见的面试题之一，所以我们今天就来盘它。@Autowired 和 @Resource 的区别主要体现在以下 5 点：

1.  来源不同；
2.  依赖查找的顺序不同；
3.  支持的参数不同；
4.  依赖注入的支持不同；
5.  编译器 IDEA 的提示不同。

## 1.来源不同

**@Autowired 和 @Resource 来自不同的“父类”，其中 @Autowired 是 Spring 定义的注解，而 @Resource 是 Java 定义的注解，它来自于 JSR-250（Java 250 规范提案）。**

> 小知识：JSR 是 Java Specification Requests 的缩写，意思是“Java 规范提案”。任何人都可以提交 JSR 给 Java 官方，但只有最终确定的 JSR，才会以 JSR-XXX 的格式发布，如 JSR-250，而被发布的 JSR 就可以看作是 Java 语言的规范或标准。

## 2.依赖查找顺序不同

依赖注入的功能，是通过先在 Spring IoC 容器中查找对象，再将对象注入引入到当前类中。而查找有分为两种实现：按名称（byName）查找或按类型（byType）查找，其中 @Autowired 和 @Resource 都是既使用了名称查找又使用了类型查找，但二者进行查找的顺序却截然相反。

### 2.1 @Autowired 查找顺序

**@Autowired 是先根据类型（byType）查找，如果存在多个 Bean 再根据名称（byName）进行查找**，它的具体查找流程如下：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-529bdc41-0576-4991-aaa7-b43ae1f55de8.jpg)

关于以上流程，可以通过查看 Spring 源码中的 org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor#postProcessPropertyValues 实现分析得出，源码执行流程如下图所示：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-60ef8c35-4122-491e-a504-6a0ee5231e90.jpg)

### 2.2 @Resource 查找顺序

**@Resource 是先根据名称查找，如果（根据名称）查找不到，再根据类型进行查找**，它的具体流程如下图所示：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-4e7ff24e-9e91-4241-8e2a-1e033351c75a.jpg)

关于以上流程可以在 Spring 源码的 org.springframework.context.annotation.CommonAnnotationBeanPostProcessor#postProcessPropertyValues 中分析得出。虽然 @Resource 是 JSR-250 定义的，但是由 Spring 提供了具体实现，它的源码实现如下：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-61077910-1fa0-41c8-b55b-6d34d2d8fdda.jpg)

### 2.3 查找顺序小结

由上面的分析可以得出：

*   **@Autowired 先根据类型（byType）查找，如果存在多个（Bean）再根据名称（byName）进行查找；**
*   **@Resource 先根据名称（byName）查找，如果（根据名称）查找不到，再根据类型（byType）进行查找。**

## 3.支持的参数不同

@Autowired 和 @Resource 在使用时都可以设置参数，比如给 @Resource 注解设置 name 和 type 参数，实现代码如下：

```
@Resource(name = "userinfo", type = UserInfo.class)

private UserInfo user;
```

但**二者支持的参数以及参数的个数完全不同，其中 @Autowired 只支持设置一个 required 的参数，而 @Resource 支持 7 个参数**，支持的参数如下图所示：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-26a01726-8c63-497c-a657-dd1d2fd22eb9.jpg)

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-be0bd572-d881-490b-8d04-2a9cc58a68f8.jpg)

## 4.依赖注入的支持不同

@Autowired 和 @Resource 支持依赖注入的用法不同，常见依赖注入有以下 3 种实现：

1.  属性注入
2.  构造方法注入
3.  Setter 注入

这 3 种实现注入的实现代码如下。

**a) 属性注入**

```
@RestController
public class UserController {
    // 属性注入
    @Autowired
    private UserService userService;

    @RequestMapping("/add")
    public UserInfo add(String username, String password) {
        return userService.add(username, password);
    }
}
```

**b) 构造方法注入**

```
@RestController
public class UserController {
    // 构造方法注入
    private UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @RequestMapping("/add")
    public UserInfo add(String username, String password) {
        return userService.add(username, password);
    }
}
```

**c) Setter 注入**

```
@RestController
public class UserController {
    // Setter 注入
    private UserService userService;

    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    @RequestMapping("/add")
    public UserInfo add(String username, String password) {
        return userService.add(username, password);
    }
}
```

其中，**@Autowired 支持属性注入、构造方法注入和 Setter 注入，而 @Resource 只支持属性注入和 Setter 注入**，当使用 @Resource 实现构造方法注入时就会提示以下错误：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-1b15b519-e690-4c36-b142-49f1310f2be5.jpg)

## 5.编译器提示不同

**当使用 IDEA 专业版在编写依赖注入的代码时，如果注入的是 Mapper 对象，那么使用 @Autowired 编译器会提示报错信息**，报错内容如下图所示：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-a631f20f-e5a9-48b9-9520-8e1ad4a661c1.jpg)

虽然 IDEA 会出现报错信息，但程序是可以正常执行的。然后，我们再**将依赖注入的注解更改为 @Resource 就不会出现报错信息了**，具体实现如下：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-930ff656-bb61-4634-95c0-144d0f3739a6.jpg)

## 总结

@Autowired 和 @Resource 都是用来实现依赖注入的注解（在 Spring/Spring Boot 项目中），但二者却有着 5 点不同：

1.  来源不同：@Autowired 来自 Spring 框架，而 @Resource 来自于（Java）JSR-250；
2.  依赖查找的顺序不同：@Autowired 先根据类型再根据名称查询，而 @Resource 先根据名称再根据类型查询；
3.  支持的参数不同：@Autowired 只支持设置 1 个参数，而 @Resource 支持设置 7 个参数；
4.  依赖注入的用法支持不同：@Autowired 既支持构造方法注入，又支持属性注入和 Setter 注入，而 @Resource 只支持属性注入和 Setter 注入；
5.  编译器 IDEA 的提示不同：当注入 Mapper 对象时，使用 @Autowired 注解编译器会提示错误，而使用 @Resource 注解则不会提示错误。

#### 参考 & 鸣谢

*   www.cnblogs.com/felordcn/p/13063802.html
*   blog.csdn.net/CPLASF\_/article/details/109225213

* * *

**微信8.0将好友放开到了一万，小伙伴可以加我大号了，先到先得，再满就真没了**

**扫描下方二维码即可加我微信啦，`2022，抱团取暖，一起牛逼。`**

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-b067eb59-eb85-4c1a-b69c-e3f3de6a7fe3.jpg)

## 推荐阅读

*   [我上线了一个炫酷的项目实战教程网站，可能有的小伙伴还不知道...](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503098&idx=1&sn=0d63e8378eb24dde2a05da686f6e97a1&scene=21#wechat_redirect)
*   [没有二十年功力，写不出这一行“看似无用”的代码！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503069&idx=1&sn=f001fc4ee18ef00182bf805e6031fbcb&scene=21#wechat_redirect)
*   [用户钱付了，订单还是显示未支付，用户炸了！聊聊如何防止支付掉单！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503064&idx=1&sn=cec67c724b65c64edb4674a6e67f4395&scene=21#wechat_redirect)
*   [5分钟自建数据库可视化平台，在线管理数据库也太方便了！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503037&idx=1&sn=47864a49b0f8ec15c3692aee140d5e4c&scene=21#wechat_redirect)
*   [程序员有很厉害，不外传的代码吗？可以运行，但不能动的那种！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503013&idx=1&sn=be4f3f3c59579accbf473be46e9e6190&scene=21#wechat_redirect)
*   [看了我常用的数据库设计技巧，同事也开始悄悄模仿了...](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503007&idx=1&sn=096ed50474b32facb92ff20f2ac12f54&scene=21#wechat_redirect)
*   [重磅更新！Mall实战教程全面升级，瞬间高大上了！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247499376&idx=1&sn=3ed28795cdd35fbaa3506e74a56703b0&scene=21#wechat_redirect)
*   [40K+Star！Mall电商实战项目开源回忆录！](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247486684&idx=1&sn=807fd808adac8019eb2095ba088efe54&scene=21#wechat_redirect)

[](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247486684&idx=1&sn=807fd808adac8019eb2095ba088efe54&scene=21#wechat_redirect)



![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-weismspringhideadftjsyautowiredzj-a92889dd-1607-41f7-a0ec-8f57712cffc0.jpg)

>参考链接：[https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503105&idx=1&sn=f57cb845f3cb401ef3e85ef622cf207e&chksm=fc2c7109cb5bf81fcc36011bc8b0d625a6ce2a51d449b1eb2fbdee2019d7e2489743abbc227d#rd](https://mp.weixin.qq.com/s?__biz=MzU1Nzg4NjgyMw==&mid=2247503105&idx=1&sn=f57cb845f3cb401ef3e85ef622cf207e&chksm=fc2c7109cb5bf81fcc36011bc8b0d625a6ce2a51d449b1eb2fbdee2019d7e2489743abbc227d#rd)，出处：macrozheng，整理：沉默王二
