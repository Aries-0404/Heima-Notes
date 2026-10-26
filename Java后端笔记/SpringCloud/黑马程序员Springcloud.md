# 黑马程序员Springcloud

学习这里的springcloud内容得用到黑马商城项目，所以我们得导入这个项目并在虚拟机中创建它的数据库等内容，所以我们需要虚拟机和docker，在docker中直接利用脚本文件和配置文件创建需要的数据库，所以我们创建了一个虚拟机Centos7_03（密码：xyy19970404），然后按照docker笔记中的步骤安装了docker，后面跟着视频中的内容在docker中利用配置文件重新安装mysql创建脚本等



## 黑马最新笔记资料请访问：

飞书链接：https://b11et3un53m.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec?from=from_copylink   
密码：j.N?-+4[



## 前置工作：数据库安装

1、首先创建一个hm-net的network，后面创建数据库的时候要用到

![image-20240526110925740](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240526110925740.png)



2、在docker中利用下载好的mysql脚本进行配置的代码（这里就用到了hm-net）：

```shell
docker run -d \
	--name mysql \
	-p 3306:3306 \
	-e TZ=Asia/Shanghai \
	-e MYSQL_ROOT_PASSWORD=123 \
	-v /root/mysql/data:/var/lib/mysql \
	-v /root/mysql/conf:/etc/mysql/conf.d \
	-v /root/mysql/init:/docker-entrypoint-initdb.d \
	--network hm-net \
	mysql
```

![image-20240526112649443](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240526112649443.png)



![image-20240526112849313](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240526112849313.png)



## 重启虚拟机注意事项

每次关闭虚拟机后其中的docker容器也会随之关闭，sqlyog没法连接上述创建的mysql容器，此时利用docker重新启动mysql容器就好（不用将上述创建mysql容器的命令再执行一遍的，否则就创建了多个mysql容器了），利用docker重新启动mysql容器之前需要重新启动docker，然后查看docker中的所有容器，找到mysql容器然后启动它

![image-20240606154524845](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240606154524845.png)



## 前端启动注意事项：

在全英文目录下打开cmd窗口，然后通过命令`start nginx.exe`启动前端页面，此时访问18080页面就打开了黑马商城的页面

![image-20240606160017200](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240606160017200.png)

![image-20240606160057453](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240606160057453.png)





## 1、微服务

### 是什么：

微服务是一种项目架构的方式，与微服务相对的就是单体架构，单体架构指的就是将一个完整的项目全部部署在一台tomcat服务器上，而微服务架构指的是将一个完整的项目按照功能拆分成多个微小的服务，不同的微服务部署到不同的tomcat服务器上，不同的微服务之间通过网络进行通信。

### 为什么：

#### （1）为什么要使用微服务架构？

因为单体架构面临一些无法解决的缺陷，所以使用微服务架构，具体介绍如下：

##### notes：关于热点功能耗尽系统资源的解释

原先将完整的项目（比如一个购物商城项目其中包含多个功能模块：用户模块、购物车模块、商品模块、订单模块等）全部部署到一台`tomcat`服务器上，现在购物高峰期假设很多人来浏览商品，就会造成`tomcat`的很多链接都被访问商品的请求占据了，此时别的比如创建订单的请求就得等候商品浏览完了空出来`tomcat`链接了才能访问（因为一个`tomcat`上的链接总数是固定的，被其中一个请求占据了以后别的请求就没法访问`tomcat`了）

![image-20241026112249139](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026112249139.png)



### 怎么办：

#### （1）如何使用微服务架构？

![image-20241026162139953](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026162139953.png)



##### notes：Tomcat 是什么？

Tomcat 是一个软件，主要用于运行和管理 Java Web 应用程序。

##### notes：为什么将 Web 项目部署到 Tomcat 上？

1. **运行环境**：Tomcat 提供了一个 Java Web 应用的运行环境，能够处理 Java Servlet 和 JSP。将 Web 项目部署到 Tomcat 上，意味着可以利用这个环境来运行和管理应用。
2. **请求处理**：Tomcat 负责处理来自客户端的 HTTP 请求，将其路由到相应的 Servlet，生成动态内容并返回给客户端。
3. **简单易用**：Tomcat 配置相对简单，适合小型和中型 Java Web 应用开发，广泛用于开发和测试阶段。

##### notes：将web项目部署到Tomcat的过程需要人为参与吗？

- springboot项目：如果你的项目是一个Spring Boot 项目，则通常内嵌 Tomcat。这种情况下，实现了tomcat的自动部署。

- 普通的javaweb项目：如果你的项目是一个传统的 Java Web 应用，通常会生成 WAR 文件，需要人为将其部署到外部的 Tomcat 服务器。

##### notes：通常说tomcat服务器，一般而言服务器指的是一台电脑，那么tomcat是一台电脑吗？

通常说“Tomcat 服务器”，这里的“服务器”指的是运行 Tomcat 的软件环境，而不是一台物理计算机。Tomcat 是一个软件，可以在物理服务器、虚拟机或容器中运行。

`Tomcat `是一个开源的 Java Servlet 容器，主要用于运行 Java Web 应用程序。

- **服务器的定义**：在计算机科学中，服务器不仅可以指代一台物理电脑，还可以指代在这台电脑上运行的服务软件。因此，“Tomcat 服务器”通常指的是运行 Tomcat 软件的计算机或虚拟机。





#### notes：如何开启springboot项目的启动项？

![image-20241024214839351](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241024214839351.png)

#### notes：在开启springboot项目的启动项的时候可以配置项目的运行环境（到底是本地运行还是在dev环境中运行，二者的数据库位置等会有区别）

在项目的`application.yaml`文件中一般写了项目的运行环境为`dev`开发环境，但是如果我们要在本地运行的话需要修改`yaml`文件，然后下次在`dev`环境中开发的话又得改回去这样太麻烦了，所以还可以在配置启动项的时候配置运行环境（配置启动项中的运行环境会覆盖`yaml`文件中的运行环境）



#### notes：为什么要配置springboot项目的启动项？

在 Spring Boot 项目中，通常在 `application.yaml` 或 `application.properties` 文件中配置开发环境、数据库连接、缓存等配置项，而在配置启动项时，你还需要额外设置某些参数，主要是为了更灵活地管理和控制项目的启动过程。具体原因有以下几点：

1. **多环境支持和切换**

Spring Boot 通过 `application.yaml` 中的不同配置文件来支持多环境（如 `dev`、`test`、`prod` 等）。比如，你可以有多个 `application-{profile}.yaml` 文件，其中包含不同环境的配置。

启动 Spring Boot 项目时，你可以通过 `spring.profiles.active` 参数来指定某个环境配置文件。例如，当你在本地开发时，可能需要激活 `dev` 环境配置，而在生产环境中，则需要激活 `prod`。

在启动项配置时显式地设置 `Active profiles`，是为了告诉应用程序启动时需要加载哪个环境的配置。这与 `yaml` 文件中的配置相辅相成。默认情况下，Spring Boot 只会加载默认的配置（如 `application.yaml`），但当你有多个环境配置时，手动指定启动环境显得尤为重要。

2. **启动项配置的灵活性**

虽然 `yaml` 文件可以定义环境相关的配置项，但启动项的配置是更灵活的一种方式：

- **开发测试的方便性**：在本地开发或调试时，配置启动项可以临时修改应用的运行方式，而不影响代码仓库中的配置文件。这特别适用于开发人员在不同的测试环境中快速切换配置。
- **命令行参数的优先级更高**：在 Spring Boot 中，启动时传递的命令行参数优先级高于配置文件中的参数。通过启动项配置，你可以覆盖 `yaml` 中的一些默认配置，这样可以根据不同的需求动态调整启动环境。

3. **避免修改代码或配置文件**

通过 IDEA 或命令行配置启动项，可以避免每次修改 `yaml` 文件。例如，如果你有一个项目需要在开发环境和测试环境之间频繁切换，而你又不想在 `yaml` 文件中来回修改配置，可以通过启动项配置临时指定激活的环境。这样，当你切换开发环境时，不需要修改项目的核心配置文件，只需在启动项中修改即可。

4. **针对特定任务的启动配置**

在开发中，启动项的配置不仅仅是用于配置环境，有时你可能需要对 JVM 参数、启动参数、应用的特定模块进行定制化配置。例如，增加调试模式 (`debug`)、调整内存分配、加载特定的类路径等。这样可以更好地进行性能调试、测试和优化。

5. **方便启动项目**

我们当然可以单独找到一个个`springboot`项目的启动类然后加载，但是这样太麻烦了，得一个个找，现在配置启动项将所有`springboot`项目的启动项集中在一起省的再去找，想启动哪一个项目直接点就可以了







## 2、springcloud

### 是什么：

是用来构建微服务架构的最常见的框架，微服务架构相比于单体架构的特点就是各个微服务部署在不同的tomcat上，所以各个微服务之间没法像在单体架构中一样可以互相调用，所以需要通过网络通信实现各个微服务之间的相互调用

### 怎么办：

#### （1）微服务项目的工程结构

![image-20241026165031464](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026165031464.png)

#### （2）如何使用springcloud构建微服务？

这里我们采用maven聚合的方式构建微服务，原先的单体项目黑马商城是一个Project，然后在其下创建了多个module，其中每个module构成一个微服务（商品微服务、购物车微服务等等）。

见下述示例，将黑马商城单体项目改成微服务项目：

##### ①拆分商品服务

###### 是什么：

将原先单体项目黑马商城中的商品模块单独拆分成一个微服务

###### 怎么办：

###### （1）如何将商品模块拆分成一个微服务？

①在当前项目project下创建一个module名为`item-service`

②引入依赖

③创建启动类

④编写配置文件

⑤将原本项目中和商品有关的Java代码复制到`item-service`下

![image-20241026165828057](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026165828057.png)

![image-20241026165925604](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026165925604.png)





##### ②拆分购物车服务

###### 是什么：

将原先单体项目黑马商城中的购物车模块单独拆分成一个微服务

###### 怎么办：

###### （1）如何将购物车模块拆分成一个微服务？

①在当前项目project下创建一个module名为`cart-service`

②引入依赖

③创建启动类

④编写配置文件

⑤将原本项目中和购物车有关的Java代码复制到`cart-service`下

![image-20241026170716555](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026170716555.png)

![image-20241026170802260](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026170802260.png)



##### ③构建微服务之间的远程调用

![image-20241026172049282](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026172049282.png)



## 3、服务注册与发现

### 是什么：

服务提供者将自己的信息（ip地址和端口号等）提交到注册中心（是springcloud的一个组件），服务调用者去注册中心订阅相关服务就可以获取到服务提供者的信息

### 为什么：

前述通过RestTemplate可以实现发送http请求但是此时的ip地址是写死的，假如碰上服务提供者采用主从架构等模式或者是当前的服务宕机了此时服务的地址换成了从机的地址，那么这个写死的地址就没法更改了

![image-20241026182340349](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026182340349.png)

### 怎么办：

#### （1）常用的注册中心：nacos

最常用的注册中心就是`nacos`

![image-20241026182734075](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026182734075.png)

#### （2）nacos的使用流程

##### ①服务提供者将信息注册到nacos

![image-20241026183601713](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026183601713.png)



##### ②服务调用者从nacos中获取服务提供者的信息（然后手动根据负载均衡算法获取服务提供者的实例，通过restTemplate向服务提供者发送http请求）

![image-20241026183718709](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026183718709.png)



### notes：前置内容，在docker中安装nacos容器

==注意这里我们已经在docker容器中设置了nacos的启动方式为：always，这意味着每次docker容器重启以后nacos都会自动重启不需要手动重启了==

```shell
<!--这是启动nacos的命令-->
docker run -d \
  --name nacos \
  --env-file ./nacos/custom.env \
  -p 8848:8848 \
  -p 9848:9848 \
  -p 9849:9849 \
  --restart=always \
  nacos/nacos-server:v2.1.0-slim
```

==notes：重点易错内容==

这里我们会在`docker`中的`mysql`容器的数据库表中存储`nacos`的数据（包括配置等内容），后续启动`nacos`的时候会去读取这个数据库当中的内容，但是由于我们设置了`nacos`的启动方式为`always`这就导致每次重启`docker`的时候`nacos`都会跟着重启，但是由于`mysql`没有设置启动方式为`always`，这就导致每次随着`docker`容器的重启`nacos`都会重启然后去读取`mysql`中的数据但是此时`mysql`没有启动所以自然无法读取数据这也就导致了`nacos`没法使用（==我们如何得知`nacos`没法使用的呢？明明通过`docker ps`命令已经看到`nacos`容器正在运行啊，但是通过`docker logs -f nacos`通过日志发现`nacos`并没有正常启动，`docker ps` 命令确实是用来列出当前正在运行的容器的。但是有时候，一个容器即使显示为“运行中”，它的某些服务或应用程序可能会因为配置或其他问题无法正常启动==），所以做法应该是首先通过命令`docker stop nacos`关闭`nacos`，然后通过命令`docker start mysql`启动`mysql`，再然后通过命令`docker start nacos`启动`nacos`此时的`nacos`就可以读取到`mysql`数据库表中的信息了。





## 4、OpenFeign

### 是什么：

用来简化微服务之间远程调用代码的工具（它也是springcloud的一个组件），现在有两个微服务A和B分别部署在不同的机器上，通过openfeign可以实现直接在微服务A中通过服务名调用微服务B中的方法，就好像两个微服务像是在单体架构中一样（其实它的底层动态代理依然是通过获取远程微服务的url地址然后建立http连接，但是动态代理的过程不需要我们手动实现所以更方便）

#### 使用示例：

以下是一个简单的 OpenFeign 使用示例：

1. **创建接口**：在微服务 A 中定义一个接口，该接口表示对微服务 B 的远程调用。

   ```java
   @FeignClient(name = "service-b")
   public interface ServiceBClient {
       @GetMapping("/api/data")
       String getData();
   }
   ```

2. **调用接口**：在微服务 A 中通过依赖注入的方式调用该接口。

   ```java
   @RestController
   public class MyController {
       @Autowired
       private ServiceBClient serviceBClient;
   
       @GetMapping("/get-data-from-b")
       public String getDataFromB() {
           return serviceBClient.getData();
       }
   }
   ```

3. **配置文件**：在微服务A的 `application.yml` 中配置微服务 B 的地址。

   ```yaml
   service-b:
     ribbon:
       listOfServers: http://localhost:8081
   ```

通过上述步骤，就可以实现微服务 A 通过 OpenFeign 调用微服务 B 的方法，仿佛两个微服务是在同一个应用程序中一样。

#### notes：这里远程调用的方法并没有方法体，它不用具体实现的吗？

这里client会远程调用别的类中的方法所以client相当于代理类，被远程调用的相当于被代理类，代理类中的方法不需要方法体，因为它会去调用被代理类中对应的方法。

例如：

```java
@FeignClient(name = "service-b")
public interface ServiceBClient {
    @GetMapping("/api/data")
    String getData();
}
```

在这个例子中，在微服务A中`getData` 方法没有方法体，但在运行时，Feign 会动态生成一个代理对象，这个对象会根据`@GetMapping("/api/data")` 的配置去调用远程服务 `service-b` 的 `/api/data` 接口，并将结果返回。所以，你可以像调用本地方法一样去调用 Feign 客户端的方法，而 Feign 会在背后处理所有的 HTTP 请求和响应处理。

### 为什么：

#### （1）为什么要使用openfeign？

在上述过程中我们利用`nacos`实现了服务注册与发现，然后使用`RestTemplate`发送`http`请求从而实现了远程调用，但是用`RestTemplate`实现远程调用的代码太麻烦了（首先得通过`nacos`的`DiscoveryClient`利用服务名获取服务的`ip`地址和端口号，然后再利用`restTemplate`发送`http`请求），所以可以利用`OpenFeign`简化远程调用（`OpenFeign`没法替代`nacos`，因为服务依然是注册在`nacos`中的，`openfeign`只是简化调用过程，依然是去`nacos`中获取消息）

### 怎么办：

#### （1）如何使用openfeign？

##### ①引入依赖（在服务调用者中引入openfeign的依赖）

##### ②启用feign（在启动类上通过注解启动）

##### ③创建feign接口，它有两个作用：（定义一个接口，作用就是从nacos中通过服务名获取服务提供者的信息(ip地址和端口号等，这个获取信息的过程是自动实现的)，接口中定义发送http请求的方法(包括请求方式、请求路径、请求参数)，这个方法不需要方法体，后续会通过动态代理直接调用远程微服务中的对应方法）

- 通过服务名从nacos中获取对应的服务提供者的信息（ip地址和端口号等，这个过程是自动实现的）
- 创建向服务提供者发送http请求的方法（包括请求方式、请求路径、请求参数），这个过程是通过动态代理自动实现的，不需要编写方法体

##### ④使用feign接口，不用去实现feign接口中的方法后续会通过动态代理实现（可以完成服务发现，负载均衡，远程调用的功能，此时就不再需要RestTemplate了）

![image-20240608101217828](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240608101217828.png)

![image-20240608101234602](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240608101234602.png)

![image-20240626110624142](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626110624142.png)

![image-20240608101340544](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240608101340544.png)

![image-20240608101438214](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240608101438214.png)



#### notes：有关feign接口发送http请求的问题？

在`openfeign`的使用过程中，我们会去创建一个`feign`接口，然后会在其中创建发送`http`请求的方法，其中请求路径是在`@GetMapping`等注解中写的，这样不会有问题吗？以前在`springmvc`中使用`@GetMapping`这些注解其中写明的`url`地址是作为与前端发送的请求进行匹配的地址，而现在是将其作为要发送的地址，这样可以吗？

==这样可以，不会有问题==。==在 OpenFeign 中，`@GetMapping` 注解用于声明**请求的路径**和方法类型（如 GET），而不是直接暴露给前端的 URL作为匹配的地址。Feign 接口的作用是充当**客户端的 HTTP 请求代理**，==因此：

1. **内部请求**：在 Feign 客户端接口中，`@GetMapping` 注解中的路径表示的是请求要发送到的目标路径，而不是外部请求的匹配路径。这个注解告诉 Feign 客户端需要向哪个微服务发送请求。
2. **服务发现和路由**：在微服务架构中，通过服务注册与发现，Feign 知道这个路径应该发送给哪个服务。例如，如果 Feign 客户端的基础路径（`@FeignClient` 注解中的 `name` 参数）指向 `item-service`，然后你在方法上定义了 `@GetMapping("/items/{id}")`，那么 Feign 会将这个请求路径和方法类型作为配置，将请求发送到 `item-service` 服务的 `/items/{id}` 这个路径。

==为什么这样可以工作？==

- **区别于传统的 `@GetMapping`**：在传统的 Spring MVC 控制器中，`@GetMapping` 注解的 URL 主要用于匹配客户端（如浏览器）发送的请求。而在 Feign 客户端中，它是用于构建和发送 HTTP 请求的目标地址。
- **Spring 的处理机制**：Spring 能够区分 Feign 客户端和控制器，因此不会将 Feign 中的 `@GetMapping` 注解误认为是控制器的 URL 路径匹配。这两种 `@GetMapping` 的用途不同，Spring 会分别处理。







### notes：通过openfeign实现远程调用和普通的远程调用有什么区别？

#### ①发送http请求时通过将ip地址写死的方式

![image-20240626101632025](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626101632025.png)

#### ②在nacos中通过服务名获取到服务实例，再获取到服务实例的uri地址，最终建立http连接的方式

![image-20240626101724616](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626101724616.png)

#### ③通过创建feign接口然后调用feign中的方法直接实现远程调用（其实feign接口中的方法依然是在nacos中通过服务名获取服务提供者的ip地址和端口号，然后通过定义的发送http请求的方法发送http请求，但是具体的实现是通过动态代理自动实现的）

![image-20240626101918351](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626101918351.png)

![image-20240626101823816](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626101823816.png)







## 5、连接池（准确来说是http连接池）

### 是什么：

`openfeign`从`nacos`中获取微服务的地址然后发送`http`请求，从而与微服务之间建立`http`连接（这里为什么不是`tcp-ip`连接或者其他连接？`tcp-ip`连接是传输层的连接而应用层的是`http`连接，`http`连接的底层是基于`tcp-ip`连接的），接着将这个连接存储在连接池中便于下次使用，不过不是所有的连接都支持放入连接池中。

### 为什么：

我们利用`openfeign`从`nacos`中获取微服务的地址然后发送`http`请求，一般而言过程就结束了，如果下次还想调用相同的微服务还得重新继续上述过程，这实现太麻烦了，所以我们想到创建连接池将暂时用不到的`http`连接保存在其中，这样下次要使用的时候直接从连接池中调用省却了再次创建的过程。

### 怎么办：

#### （1）支持放入连接池中的http连接？

![image-20241024225606518](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241024225606518.png)

#### （2）如何使用连接池？

##### ①引入依赖

![image-20241026221301521](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026221301521.png)

##### ②在配置文件中开启连接池

![image-20241026221318988](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026221318988.png)

##### ③使用连接池中的连接（这里并不需要我们特意去调用连接池中的连接，而是如果我们要建立的http连接在连接池中有实例的话它会自动进行调用的）

![image-20241026221357268](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241026221357268.png)





## 6、openfeign的使用示例

![image-20241024230210245](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241024230210245.png)

#### notes：分析上述过程中抽取微服务feign接口的两种方式

![image-20241024232624723](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241024232624723.png)

- 方式1：将商品微服务的feign接口定义到了公共的api模块中，后续购物车微服务要调用商品微服务的时候只要引入公共的api模块的依赖就可以了
- 方式2：在商品微服务模块下再创建一个商品的api子模块，再将商品微服务的feign接口定义在该api子模块中（商品微服务本身就是一个模块，在其下再创建一个api子模块），后续购物车微服务要调用商品微服务的时候只要引入商品微服务模块下的api子模块依赖就行了





## 7、日志配置

![image-20241024233517995](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241024233517995.png)



### notes：这里配置接口日志级别的配置类上没有使用@Configuration注解（所以这个配置类此时是不生效的），这是为什么？

- **全局生效**：使用 `@Configuration` 注解的配置类会被 Spring 容器识别为一个全局配置类，在该配置类中所有定义的 `@Bean` 实例都会作为单例注册到 Spring 的上下文中，可以被整个应用访问。
- **自动扫描**：在 Spring Boot 中，`@Configuration` 注解的类会被自动扫描（如果位于启动类的同包或子包中），并加入到 Spring 的上下文中。Spring 会在应用启动时加载这些配置类，从而使配置全局生效。
- **局部配置的不同**：对于一些只想作用于特定组件的配置（如 Feign 的局部配置），通常不会使用 `@Configuration` 注解，以避免全局生效。相反，使用 `@Bean` 定义的类或直接在特定组件上引用配置类，可以实现更加精细的控制。







## 8、网关

### 是什么：

网关就是**网络的关口**，网关本身也是一个微服务。数据在网络间传输，从一个网络传输到另一网络时就需要经过网关来做数据的**路由转发**以及数据**身份校验**。

更通俗的来讲，网关就像是以前园区传达室的大爷。

- 外面的人要想进入园区，必须经过大爷的认可，如果你是不怀好意的人，肯定被直接拦截。
- 外面的人要传话或送信，要找大爷。大爷帮你带给目标人。

![image-20240610152411725](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610152411725.png)

![image-20240610152443921](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610152443921.png)



### 为什么：

#### （1）为什么需要使用网关？

使用网关在微服务架构中带来诸多优势，主要体现在以下几个方面：

1. **统一入口简化前端访问**：每个微服务都有自己的地址和端口，前端在访问这些服务时需要区分并维护多个入口，这增加了开发和维护的复杂性。通过引入网关，前端只需调用网关的单一地址，网关会根据路由规则将请求转发至具体的微服务，简化了前端的接入和管理。
2. **动态服务发现**：前端无法直接调用服务注册中心（==只有后端工程才能直接访问`nacos`==）来获取实时的服务列表，因此也难以动态感知服务的上下线变化。网关可以与`Nacos`等服务发现组件集成，实现动态的服务发现和负载均衡，确保请求被路由至可用的后端实例。
3. **集中身份校验与安全控制**：在业务需求中，诸如购物车、订单等模块需要身份校验和权限验证。将身份校验功能集成到网关中，前端请求统一在网关层完成身份校验后再转发到具体的微服务，这样不仅减少了每个微服务独立实现身份校验的工作量，也提高了系统的安全性和一致性。





### 怎么办：

#### （1）如何使用网关？

![image-20241028221308662](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241028221308662.png)



#### notes：每一个配置文件中的内容都有一个Java类与之对应（需要将配置文件中的内容转换成Java类中的属性），与网关路由配置文件对应的Java类是`RouteDefinition`，这个类是自动定义好的，我们只要按照标准在配置文件中写好路由就行了

- ![image-20240610152919812](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610152919812.png)



#### （2）如何利用网关实现登录校验？

##### 为什么：

单体架构时我们只需要完成一次用户登录、身份校验，就可以在所有业务中获取到用户信息。而微服务拆分后，每个微服务都独立部署，不再共享数据。也就意味着每个微服务都需要做登录校验，这显然不可取。我们的登录是基于JWT来实现的，校验JWT的算法复杂，而且需要用到秘钥。如果每个微服务都去做登录校验，这就存在着两大问题：

- 每个微服务都需要知道JWT的秘钥，不安全
- 每个微服务重复编写登录校验代码、权限校验代码，麻烦

##### 怎么办：

##### （1）如何利用网关实现登录校验？

###### ①登录校验的流程

![image-20240610155700563](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610155700563.png)

###### ②网关过滤器

![image-20240610155917328](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610155917328.png)

如图中所示，最终请求转发是有一个名为`NettyRoutingFilter`的过滤器来执行的，而且这个过滤器是整个过滤器链中顺序最靠后的一个。**如果我们能够定义一个过滤器，在其中实现登录校验逻辑，并且将过滤器执行顺序定义到**`NettyRoutingFilter`**之前**，这就符合我们的需求了！

###### notes：常用的网关过滤器

- **`GatewayFilter`**：路由过滤器，作用范围比较灵活，可以是任意指定的路由`Route`. 
- **`GlobalFilter`**：全局过滤器，作用范围是所有路由，不可配置。

在过滤器中定义了很多现成的方法，比如增加请求头的内容等，但是我们一般需要定制化服务比如完成登录校验等此时没有现成的方法，所以只好自定义过滤器

###### ③自定义GlobalFilter过滤器，实现身份校验逻辑

也可以自定义GatewayFilter过滤器但是更麻烦

![image-20240610161603209](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610161603209.png)

![image-20240610161636919](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610161636919.png)

![image-20240610161749588](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610161749588.png)

```java
@Component
@RequiredArgsConstructor
@EnableConfigurationProperties(AuthProperties.class)
public class AuthGlobalFilter implements GlobalFilter, Ordered {
    private final JwtTool jwtTool;
    private final AuthProperties authProperties;
    private final AntPathMatcher antPathMatcher = new AntPathMatcher();

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取Request
        ServerHttpRequest request = exchange.getRequest();
        // 2.判断是否不需要拦截
        if(isExclude(request.getPath().toString())){
            // 无需拦截，直接放行
            return chain.filter(exchange);
        }
        // 3.获取请求头中的token
        String token = null;
        List<String> headers = request.getHeaders().get("authorization");
        if (!CollUtils.isEmpty(headers)) {
            token = headers.get(0);
        }
        // 4.校验并解析token
        Long userId = null;
        try {
            userId = jwtTool.parseToken(token);
        } catch (UnauthorizedException e) {
            // 如果无效，拦截
            ServerHttpResponse response = exchange.getResponse();
            response.setRawStatusCode(401);
            return response.setComplete();
        }

        // TODO 5.如果有效，传递用户信息
        System.out.println("userId = " + userId);
        // 6.放行
        return chain.filter(exchange);
    }

    private boolean isExclude(String antPath) {
        for (String pathPattern : authProperties.getExcludePaths()) {
            if(antPathMatcher.match(pathPattern, antPath)){
                return true;
            }
        }
        return false;
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

![image-20240610161943538](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610161943538.png)



#### （3）微服务如何获取用户信息

现在，网关已经可以完成登录校验并获取登录用户身份信息。但是当网关将请求转发到微服务时，微服务又该如何获取用户身份呢？

==由前端发送`http`请求到网关，然后网关进行路由转发到对应的微服务，同时微服务之间也会远程调用，这几个过程对应的不是同一个`http`请求而是新的`http`请求==。因此我们可以在网关中将用户信息以请求头的方式传递到下游微服务。然后微服务可以从请求头中获取登录用户信息（每个微服务通过在`controller`层的方法中通过请求头中的字段名获取对应的请求头信息，但是因为这样需要给每个要获取用户信息的`controller`方法定义，实在太麻烦了），因此我们可以利用`SpringMVC`的拦截器来实现登录用户信息获取（通过`springmvc`的拦截器可以统一拦截请求获取用户信息），并存入`ThreadLocal`，方便后续使用。

因此，接下来我们要做的事情有：

- 改造网关过滤器，在获取用户信息后保存到请求头，转发到下游微服务
- 编写微服务拦截器，拦截请求获取用户信息，保存到`ThreadLocal`后放行（==因为会有多个微服务需要获取用户信息，如果在每一个微服务当中都定义拦截器就太麻烦了，所以我们将拦截器定义在`common`模块下，后续某个微服务获取用户信息时直接引用`common`模块就行了==）

##### ①网关保存用户到请求头中

![image-20240610165828376](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610165828376.png)

##### ②定义拦截器从请求头中获取用户

我们需要在拦截器中定义获取用户、保存用户的代码，这里有定义好的工具类`UserContext`，我们直接引用

![image-20240610173042173](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610173042173.png)



##### notes：定义好拦截器以后还需要注册才能使用

一般是使用配置类进行注册（还可以通过在拦截器上添加`@Component`注解等方式进行注册）

![image-20240610173426664](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610173426664.png)



##### notes：关于定义的`springmvc`的拦截器和微服务不在同一个包下该如何处理？

如果拦截器所在的包和微服务所在的包不同，且没有正确配置包扫描路径，这样当微服务启动的时候`springmvc`拦截器默认情况下是不会生效的。为了确保拦截器在微服务启动时生效，需要确保微服务能够扫描到拦截器所在的包。可以通过以下几种方式解决这个问题：

###### 方式一：在微服务启动类上使用 `@ComponentScan` 注解，明确指定拦截器的包路径。

但是我们一般不使用这种方式，因为假如有多个微服务需要调用拦截器的话那么需要在每个微服务启动类上都添加@ComponentScan注解，太麻烦了

==步骤1：在拦截器类上添加 `@Component` 注解，从而将拦截器作为Spring Bean注入到Spring上下文中。==

```java
package com.hmall.common.interceptors;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

@Component
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 在请求处理之前执行的逻辑
        return true;
    }

    // 其他拦截器方法，如 postHandle, afterCompletion
}
```

==步骤2：在微服务的启动类上使用 `@ComponentScan` 注解，指定拦截器的包路径==

假设拦截器在 `com.hmall.common.interceptors` 包下，而微服务的启动类在 `com.hmall.cart` 包下。

```java
package com.hmall.cart;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = {"com.hmall.cart", "com.hmall.common.interceptors"})
public class CartServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(CartServiceApplication.class, args);
    }
}
```

##### notes：注意拦截器的注册可以通过`@Component`注解的方式和配置类中`Bean`方法的方式，这里使用的是`@Component`注解的方式然后在微服务的启动类上添加`@ComponentScan`注解和拦截器所在的包就好，还可以通过配置类中`Bean`方法的方式注册拦截器然后在微服务的启动类上添加`@ComponentScan`注解和配置类所在的包就好





###### 方式二：在拦截器所在包的 `META-INF/spring.factories` 文件中声明配置类，使其在应用启动时自动加载

==步骤1：创建配置类并注册拦截器==

```java
package com.hmall.common.interceptors;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

//这里先是创建拦截器，还没有注册，这里不用添加@Component注解实现注册，而是通过在配置类中实现注册（二者是一个效果）
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 在请求处理之前执行的逻辑
        return true;
    }

    // 其他拦截器方法，如 postHandle, afterCompletion
}
```

```java
package com.hmall.common.config;

import com.hmall.common.interceptors.UserInfoInterceptor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserInfoInterceptor());//在配置类中实现注册拦截器
    }
}
```

==步骤2：在 `META-INF/spring.factories` 文件中声明配置类==

```java
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.hmall.common.config.MvcConfig
```

==步骤3：在微服务中引入拦截器所在的公共模块依赖==

确保微服务引入了公共模块的依赖：

```xml
<dependency>
    <groupId>com.hmall</groupId>
    <artifactId>hm-common</artifactId>
    <version>1.0.0</version>
</dependency>
```





##### ③openfeign传递用户信息（通过openfeign的拦截器接口将用户信息保存到请求头中）

![image-20240610180123370](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610180123370.png)



#### notes：在maven项目中不同的module之间互相都是独立的那么该如何互相导入对方？

这就像买东西回来用一样，你必须先把东西买回来（在`pom.xml`文件中引入依赖）接着才能使用（通过`import`导入）

![image-20240626160627609](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240626160627609.png)





## 9、配置管理

### 是什么：

在 Spring Cloud 中，配置管理是指通过集中式配置服务来管理和维护分布式系统中各个微服务的配置。

### 为什么：

#### （1）为什么要进行配置管理？

到目前为止我们已经解决了微服务相关的几个问题：

- 微服务远程调用
- 微服务注册、发现
- 微服务请求路由、负载均衡
- 微服务登录用户信息传递

不过，现在依然还有几个问题需要解决：

- 每个微服务都有很多重复的配置，维护成本高（配置无法共享）
- 某些业务配置在配置文件中写死了（比如说购物车的数量上限等），每次修改都要重启服务（配置无法热更新）

- 网关路由在配置文件中写死了，如果变更必须重启微服务（配置无法进行动态路由）
  - 如果是在代码中将路由配置写死，则发生变更后需要将项目重新打包然后重启
  - 如果是在配置文件中将路由配置写死，则配置文件发生变更后不需要将项目重新打包但是依旧需要重启


所以我们就希望可以有一种方式统一进行配置的管理（避免各个微服务中重复的配置），同时可以保证配置热更新（即更改完配置以后不需要重启就能生效，这个功能很重要，在实际开发过程中更改配置以后不可能将项目重启的这样太麻烦了）。



### 怎么办：

#### （1）如何实现微服务统一配置管理？

`nacos`不仅是注册中心还可以进行统一的配置管理，通过`nacos`可以实现：

- 配置共享（将一些微服务共用的配置文件内容抽取出来在nacos中进行统一管理，这样就不用在每个微服务中单独进行配置了）
- 配置热更新（简单来说就是在nacos中更改微服务的配置文件以后不用重启就能让新的配置生效）
- 动态路由（简单来说就是在nacos中对路由配置文件进行更新）

##### ①配置共享

![image-20241029091559050](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029091559050.png)



##### notes：注意读取nacos中配置文件的过程发生在读取微服务项目中application.yaml文件的过程之前

- 这就会带来一个问题，那就是`nacos`的位置是配置在`application.yaml`文件中的，现在没有读取该`yaml`文件就要访问`nacos`，这该如何访问呢？就需要用到`bootstrap.yaml`文件（它的访问顺序发生在读取`nacos`配置文件之前），在里面写上`nacos`的地址。
- 还有一个问题，既然读取`nacos`中配置文件的过程发生在读取微服务项目中`application.yaml`文件的过程之前，那么当初在微服务远程调用的时候我们是如何访问`nacos`的呢，好像并没有配置`bootstrap.yaml`文件啊？因为远程调用的时候我们读取`nacos`不是去读取其中的配置文件而是读取注册中心中的文件，而访问注册中心的顺序发生在读取微服务的`application.yaml`文件之后。



##### ②配置热更新

![image-20241029092701839](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029092701839.png)



##### ③动态路由

**动态路由**是指在运行时，根据配置的变化动态调整路由规则，将请求转发到相应的微服务，从而能够让网关在不重启的情况下实时更新路由配置。原先是将路由配置写在网关微服务的配置文件中的，但是这种方式没法进行路由的动态更新所以现在改成将路由配置写在`nacos`中从而实现路由的动态更新。

![image-20241029093148780](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029093148780.png)

![image-20241029093319861](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029093319861.png)





## 10、微服务保护

### notes：雪崩问题（微服务级联失败问题）

在微服务架构中，"雪崩问题"（也称为“级联故障”）是一个重要的概念，它描述的是一个系统中的小的问题如何迅速扩散至整个系统，导致整个系统的性能急剧下降或完全失效。

举例：查询购物车的业务，假如商品服务业务并发较高，占用过多`Tomcat`连接。可能会导致商品服务的所有接口响应时间增加，延迟变高，甚至是长时间阻塞直至查询失败。此时查询购物车业务需要查询并等待商品服务的查询结果，从而导致查询购物车业务的响应时间也变长，甚至也阻塞直至无法访问。而此时如果查询购物车的请求较多，可能导致访问购物车服务的`Tomcat`连接占用较多，所有接口的响应时间都会增加，整个服务性能很差， 甚至不可用。

![image-20240610222526281](%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20240610222526281.png)

### 是什么：

保证服务运行的健壮性，避免级联失败导致的雪崩问题，就属于微服务保护。

### 为什么：

#### （1）为什么要实施微服务保护？

在微服务架构中微服务之间会互相调用形成错综复杂的网络结构，如果某一个微服务发生故障顺带着影响了所有和它相关的微服务一起故障从而造成整个微服务架构不可用，这是不可接受的，所以必须得采取微服务保护措施避免这种情况。

### 怎么办：

#### （1）常见微服务保护方案

微服务保护的方案有很多，比如：

##### ①请求限流：

限制微服务的访问流量（比如说现在某个微服务每秒钟有500个访问请求，我们可以限制只有50个请求能访问，其他的请求都被拒绝）

![image-20241029223045556](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029223045556.png)

##### ②线程隔离：

控制微服务内部对其他微服务进行远程调用的线程资源数目，从而将故障微服务的影响降低到最小（比如说现在商品微服务发生了故障，然后购物车微服务需要去调用它，在原始高并发的情况下在购物车微服务内部会有越来越多的线程资源堵塞在调用商品微服务这个过程中，而现在我们通过限制购物车微服务内部对于商品微服务的调用线程数目比如说为10，以此购物车微服务内部最多就只有10个线程资源因为调用商品微服务被阻塞而不会影响到购物车微服务内部其他的线程资源，从而将商品微服务的影响范围降低）

![image-20241029223331840](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029223331840.png)

##### notes：请求限流和线程隔离的区别

1. 二者的应用场景不同

- 请求限流：为了应对对某一个微服务的突发流量飙升情况
- 线程隔离：限制微服务内部对于另一个故障微服务进行远程调用的线程资源数目从而将该故障微服务的影响范围降低到最小

2. 二者的应用对象不同

- 请求限流：限制的是外部对一个微服务的调用请求（即限制的是微服务的tomcat连接资源）
- 线程隔离：限制的是一个微服务内部对于另一个微服务进行远程调用的线程资源

###### notes：线程资源和调用请求之间有什么关系？

在微服务架构中，一个服务（比如购物车服务）需要调用另一个服务（比如商品服务）时，这个调用请求是由购物车微服务中的线程资源发出的。

3. 二者的应用层次不同

- 请求限流：通常在负载均衡器、api网关或服务入口处实现。例如，在nginx、spring cloud gateway等组件中配置限流策略。
- 线程隔离：通常在微服务内部实现，通过配置独立的线程池进行管理。

##### notes：请求限流和线程隔离的示例：

假设有一个电商网站的支付服务：

1. 请求限流：在api网关处配置限流策略，限制每秒最多处理500个支付请求，放置突发流量导致支付服务过载。
2. 线程隔离：支付服务内部使用一个独立的线程池来处理支付请求，每个线程池最多并发处理20个支付请求，防止支付处理影响到其他服务。

##### ③服务熔断：

对于故障微服务，线程隔离虽然可以限制故障微服务影响的规模，但是去调用该故障微服务的线程终究阻塞在那里，为了避免这种情况我们可以直接不去访问故障微服务而是返回一个默认的结果（这个默认的结果就叫做降级逻辑），等到该服务从故障中恢复才接着去调用它。

![image-20241029223416311](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029223416311.png)



#### （2）Sentinel

##### 是什么：

是一个微服务保护框架，通过该框架我们可以对各个微服务实施请求限流、线程隔离、服务熔断等保护措施

##### 为什么：

###### （1）为什么要使用sentinel？

前面我们介绍的各种微服务保护措施，请求限流、线程隔离、服务熔断等，如果要我们自己通过代码去实现那实在太难了，索性通过`sentinel`框架我们可以在控制台上通过按钮来实现微服务保护措施。

##### 怎么办：

###### （1）如何使用sentinel？

###### ①前置处理

![image-20241029230507249](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029230507249.png)



###### ②使用sentinel实现请求限流

![image-20241029230635136](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029230635136.png)



###### ③使用sentinel实现线程隔离

![image-20241029230745678](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029230745678.png)



###### ④使用sentinel实现服务熔断

![image-20241029231157905](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029231157905.png)

##### notes：降级逻辑

当微服务故障时，跳过调用该微服务直接返回一个默认结果，这个默认的结果就是降级逻辑，不光服务熔断的时候可以调用降级逻辑，在请求限流的时候那些被限制的请求也可以返回降级逻辑，比如现在每秒有500个访问请求限流为每秒50个请求，那么剩下的那450个请求就可以返回降级逻辑





## 11、分布式事务

### 是什么：

在微服务架构中一个完整的事务可能涉及到多个微服务，这些微服务部署在不同的`tomcat`上，占据不同的数据库资源，现在要保证该事务执行前后各个微服务的数据库中保持`acid`特性，这就是分布式事务的内容。

### 为什么：

#### （1）为什么要使用分布式事务？

![image-20241029232307067](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029232307067.png)

### 怎么办：

#### （1）如何实现分布式事务？

可以通过seata框架实现分布式事务

#### （2）seata

##### 是什么：

是一个框架可以用来实现分布式事务

##### 怎么办：

###### （1）如何使用seata框架实现分布式事务？（这里举的例子时seata的at模式）

![image-20241029233005845](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029233005845.png)

![image-20241029233100249](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029233100249.png)



###### （2）为什么使用seata可以实现分布式事务？（这些属于底层原理）

![image-20241029233358964](./%E9%BB%91%E9%A9%AC%E7%A8%8B%E5%BA%8F%E5%91%98Springcloud.assets/image-20241029233358964.png)





#### notes：Redis事务和Redis的分布式锁有什么区别和联系？

共同点：都是用来确保数据一致性的

区别：

- redis事务：是一组==有关redis数据库的操作指令的集合==，用`Mutil Exec Discard Watch`构成redis事务，redis事务中的操作指令要么全部成功要么全部失败。
- redis分布式锁：在高并发场景下多个线程都要求操作redis中的同一个数据，用`Set key value NX EX timeout`指令构成redis的分布式锁，==采用redis分布式锁可以保证在同一时刻只能有一个线程操作redis中的数据==。

举例：

##### Redis事务示例

假设我们有一个银行账户系统，需要从一个账户转账到另一个账户。在这个操作中，我们需要确保两步操作要么全部成功，要么全部失败：从源账户扣款，并向目标账户存款。我们可以使用Redis事务来保证这两个操作的原子性。

```java
import redis.clients.jedis.Jedis;

public class RedisTransactionExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost");

        String sourceAccount = "account:source";
        String targetAccount = "account:target";

        // 监视源账户的余额
        jedis.watch(sourceAccount);

        // 开启事务
        jedis.multi();

        // 从源账户扣款
        jedis.decrBy(sourceAccount, 100);

        // 向目标账户存款
        jedis.incrBy(targetAccount, 100);

        // 提交事务
        List<Object> result = jedis.exec();

        if (result == null) {
            System.out.println("Transaction failed due to concurrent modification.");
        } else {
            System.out.println("Transaction succeeded.");
        }

        jedis.close();
    }
}
```

##### Redis分布式锁示例

假设我们有一个库存系统，多个线程或进程可能同时尝试减少某个商品的库存。为了防止多个线程或进程同时修改库存，我们可以使用Redis分布式锁来确保只有一个线程或进程能够在某一时刻修改库存。

```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.params.SetParams;

public class RedisDistributedLockExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost");

        String lockKey = "lock:product:12345";
        String lockValue = UUID.randomUUID().toString();
        SetParams setParams = new SetParams().nx().px(30000);

        // 尝试获取锁
        String result = jedis.set(lockKey, lockValue, setParams);

        if ("OK".equals(result)) {
            try {
                // 执行需要加锁的操作，比如减少库存
                System.out.println("Acquired lock, performing operation...");
                jedis.decrBy("product:12345:stock", 1);
            } finally {
                // 释放锁
                String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
                jedis.eval(script, Collections.singletonList(lockKey), Collections.singletonList(lockValue));
                System.out.println("Released lock.");
            }
        } else {
            System.out.println("Failed to acquire lock, another process is working on it.");
        }

        jedis.close();
    }
}
```





#### notes：Redis事务和Springcloud中的分布式事务有什么区别和联系？

共同点：都是用来保证数据一致性的

区别：

- redis事务：是一组==有关redis数据库的操作指令的集合==，用`Mutil Exec Discard Watch`构成redis事务，redis事务中的操作指令要么全部成功要么全部失败。
- springcloud分布式事务：一组==横跨多个微服务和数据源（包括但不限于Redis、关系数据库、消息队列等）的操作指令的集合==，分布式系统中多个服务的操作要么全部成功要么全部回滚。通过分布式事务管理器（如Seata、Atomikos、Narayana等）和事务协调协议（如两阶段提交（2PC）或Saga模式）实现springcloud分布式事务，==操作类型包括数据库操作、非数据库操作（如调用外部API、消息队列等）==。

##### Redis事务示例

假设我们有一个银行账户系统，需要从一个账户转账到另一个账户。在这个操作中，我们需要确保两步操作要么全部成功，要么全部失败：从源账户扣款，并向目标账户存款。我们可以使用Redis事务来保证这两个操作的原子性。

```java
import redis.clients.jedis.Jedis;

public class RedisTransactionExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost");

        String sourceAccount = "account:source";
        String targetAccount = "account:target";

        // 监视源账户的余额
        jedis.watch(sourceAccount);

        // 开启事务
        jedis.multi();

        // 从源账户扣款
        jedis.decrBy(sourceAccount, 100);

        // 向目标账户存款
        jedis.incrBy(targetAccount, 100);

        // 提交事务
        List<Object> result = jedis.exec();

        if (result == null) {
            System.out.println("Transaction failed due to concurrent modification.");
        } else {
            System.out.println("Transaction succeeded.");
        }

        jedis.close();
    }
}
```

##### Spring Cloud分布式事务示例

假设我们有一个订单系统，涉及到订单服务和支付服务。创建订单时，需要确保订单创建成功的同时，支付也成功。这是一个跨多个微服务的事务，Spring Cloud中的分布式事务可以帮助我们实现这一点。

我们使用Spring Boot和Seata来实现分布式事务（==具体Seata的使用不介绍了==）。假设我们有两个微服务：OrderService和PaymentService。

==OrderService==

在OrderService中通过@GlobalTransactional注解创建分布式事务，确保订单创建和支付操作要么全部成功，要么全部失败。

```java
@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private OrderService orderService;

    @PostMapping("/create")
    public String createOrder(@RequestBody Order order) {
        return orderService.createOrder(order);
    }
}

@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private PaymentService paymentService;

    @GlobalTransactional
    public String createOrder(Order order) {
        orderRepository.save(order);
        paymentService.pay(order.getId(), order.getAmount());
        return "Order created and payment successful";
    }
}

@FeignClient(name = "payment-service")
public interface PaymentService {
    @PostMapping("/payment/pay")
    void pay(@RequestParam("orderId") Long orderId, @RequestParam("amount") BigDecimal amount);
}
```

==PaymentService==

```java
@RestController
@RequestMapping("/payment")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;

    @PostMapping("/pay")
    public void pay(@RequestParam("orderId") Long orderId, @RequestParam("amount") BigDecimal amount) {
        paymentService.pay(orderId, amount);
    }
}

@Service
public class PaymentService {

    @Autowired
    private PaymentRepository paymentRepository;

    @Transactional
    public void pay(Long orderId, BigDecimal amount) {
        Payment payment = new Payment();
        payment.setOrderId(orderId);
        payment.setAmount(amount);
        paymentRepository.save(payment);
    }
}
```





#### notes：Redis中的分布式锁和Springcloud中的分布式事务有什么区别和联系？

共同点：都是用来保证数据一致性的

不同点：

- redis分布式锁：在高并发场景下多个线程都要求操作redis中的同一个数据，用`Set key value NX EX timeout`指令构成redis的分布式锁，==采用redis分布式锁可以保证在同一时刻只能有一个线程操作redis中的数据==。
- springcloud分布式事务：一组==横跨多个微服务和数据源（包括但不限于Redis、关系数据库、消息队列等）的操作指令的集合==，分布式系统中多个服务的操作要么全部成功要么全部回滚。通过分布式事务管理器（如Seata、Atomikos、Narayana等）和事务协调协议（如两阶段提交（2PC）或Saga模式）实现springcloud分布式事务，==操作类型包括数据库操作、非数据库操作（如调用外部API、消息队列等）==。