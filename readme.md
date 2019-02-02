# 效果

创建了一个 eureka server,一个 eureka client,成功注册上.

# 参考

[SpringCloud2.0入门指南](https://www.kancloud.cn/fymod/springcloud2/784129)

说的很详细,就不多言了.

# 过程

idea 提供了快速创建.

Spring Initializr -> Cloud Discovery -> Eureka Server : eureka server

Spring Initializr -> Cloud Discovery -> Eureka Discovery client

[Spring Cloud Eureka 快速创建Client和Server ](https://my.oschina.net/u/3747963/blog/1593462)

截图不放了,看这篇.

---

# spring cloud 是啥

spring cloud 是一个微服务框架,提供分布式系统操作的开发工具.

那啥是微服务?

学习 web 时会接触到三层架构之类的...

控制层,服务层,数据层...

我们可以把这种抽象当作 web 的纵向分层.而微服务,就相当于对 web 进行横向分割.

比如一个大型应用,我们之前分为控制,服务,数据...来开发.

现在,我们可以先把这个应用横向,以**功能拆分**为几个模块(不同的服务),每个服务再纵向抽象来开发.

再详细点,比如开发一个购书网站.我们现在就可以先划分为用户模块,书籍模块...甚至更细一点,一些优惠活动一个模块...而单独的每一个模块,都可以用 spring boot 等方式来开发.

每个模块(服务)都可以独立部署.(部署方式很多,了解一下 **docker**.)而分布式,可以先简单地理解为把服务部署到不同的服务器上?

嗯,这就是微服务...大概.

在前后端分离时就会大概有这种感觉,那就是我们并不关心数据,操作的来源,后端给前端提供 api 就行了.用微服务就相当于对 api 进行模块化,每个服务提供相应的 api.便于组织,也便于复用.(当然是在业务达到一定复杂度的情况下)

事实上,spring cloud 也建议服务间通过诸如 RESTful API 等方式调用.

spring cloud 基于 spring boot,用 spring cloud 搭建的微服务应用中的各个模块(服务),用 spring boot 开发.(用其他方式行不行呢?)

# spring cloud 基本组织

把应用拆分为多个服务,我们可以手动来组织这些服务间的调度.这称为**服务治理**.不过可想而知,这样效率很低,我们仍然需要一个中心来管理调度这些服务.(实现各个微服务实例的自动化注册与发现)

这个中心被称为**服务注册中心**.

spring cloud 为服务治理做了一层抽象接口,支持多种不同的服务治理框架,如 zookeeper,(PS : 了解 Dubbo 时就是用的这个)Netflix Eureka...

网飞牛皮!

而各种微服务呢,被分为**服务提供者**和**服务消费者**.一个提供接口,一个使用接口...

这也是抽象层面的说法,在 spring cloud 的各种教程中,都选用 Eureka 作为服务注册中心,我们以 Eureka 来具体谈谈...

> 先总结一下,微服务包含这三者...
>
> 1. 服务注册中心
> 2. 服务提供者
> 3. 服务消费者

# Eureka 简单使用

## 参考

这篇写的很详细了...

[SpringCloud2.0 入门指南](https://www.kancloud.cn/fymod/springcloud2/784129)

## 正文

Eureka 分为 Eureka Server,Eureka Client.

可以用 idea 快速创建.

Eureka Server : Spring Initializr -> Cloud Discovery -> Eureka Server

Eureka Client : Spring Initializr -> Cloud Discovery -> Eureka Discovery client

[Spring Cloud Eureka 快速创建Client和Server ](https://my.oschina.net/u/3747963/blog/1593462)

截图就不放了,看这篇.

具体怎么操作以及有些注意事项之类的...直接看上面的 [SpringCloud2.0 入门指南](https://www.kancloud.cn/fymod/springcloud2/784129) !!!

不过我喜欢用 gradle,我是这样创建的:

1. 创建一个 gradle 项目(springcloud-demo),然后把里面内容删光光.
2. 在这个项目引入 Module(idea 上右键项目名,new -> Module),Spring Initializr -> Cloud Discovery -> Eureka Server.Module 命名为 eurekaserver.这样,创建一个 Eureka Server.修改配置之类的就不提了.
3. 在这个项目引入 Module(idea 上右键项目名,new -> Module),Spring Initializr -> Cloud Discovery -> Eureka Discovery client.Module 命名为 eurekaserver.这样,创建了一个 Eureka Client.修改配置之类的就不提了.

执行 gradle 导入包,启动这两个 Module,最后在网页打开 Eureka Server,成功发现了 Eureka Client.

## 代码仓库

[springcloud-demo:basic-demo](https://github.com/hqweay/springcloud-demo/tree/basic-demo)

## gradle 出现错误

gradle 导包 eureka-client 一直失败.

设置, gradle 中 取消 offline work(离线工作设置),导包成功...

见 : [IDEA + Gradle导包，莫名的导包失败~~](https://blog.csdn.net/u013972652/article/details/80182838)

这玄学啊...

# Eureka Server 和 Eureka Client 到底啥区别

Eureka Server 是注册服务中心的服务端,有界面.

Eureka Client 是注册服务中心的客户端.

这啥意思...

对比前文.

> 微服务包含这三者.
>
> 1. 服务注册中心
> 2. 服务提供者
> 3. 服务消费者

Eureka Server 就是服务注册中心.

Eureka Client 既可以做服务提供者,又可以做服务消费者.不过在前文,其实是默认做服务提供者了.

那么 Eureka Client 如何做消费服务呢?

[SpringCloud Ribbon客户端负载均衡](https://www.kancloud.cn/fymod/springcloud2/784130)

服务消费者,也就是作为接口的调用层,比如 web.

例如,新闻 web 调用热点新闻服务的接口,获取当日热点新闻...

好了,如何做呢?

用 Ribbon...

好了,这是后面的内容了...不提了...

# 高可用注册中心

Eureka Client 既可以做服务提供方,又可以做服务消费方,为什么这样设计?

实际上,Eureka 中的所有节点都既是服务提供方又是服务消费方,包括 Eureka Server.

这样设计是因为考虑到高可用问题,而 Eureka 的高可用的实现方式,就是把 Eureka Server 作为一个服务,向其它的服务注册中心注册.

这样就可以形成一组互相注册的服务注册中心,以实现服务清单的互相同步,达到高可用的效果.

[Spring Cloud 微服务实战]

## 我的理解

一个服务注册中心维护一堆服务,如果两个服务注册中心互相注册(并且同步),那么他们各自都得到了扩展!可以使用别人的服务啦!

但是但是!!!!

实际上的意思似乎是,一个 Eureka Client 同时向两个 Eureka Server 注册.这样,如果一个 Eureka Server 挂了,还有另一个 Server 可以用!

[SpringCloud之高可用的服务注册中心（Eureka）](https://blog.csdn.net/yelllowcong/article/details/79585816)

over...

一家之言,若有错误,还请斧正...