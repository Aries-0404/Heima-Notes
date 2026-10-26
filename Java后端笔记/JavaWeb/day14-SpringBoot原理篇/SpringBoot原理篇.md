# SpringBoot原理篇

==这里仅仅是一个小的总结，要看详细的还是去看pdf文档==



## 1、SpringBoot最大的特点

### （1）依赖管理

#### 是什么：

通过各种场景启动器starter，将某一个开发场景下所需要的全部依赖包含进去，后续只要引入starter依赖就可以将相关的依赖全部引入了

#### 怎么办：

##### （1）如何做到将所有的依赖包含进去的？

通过maven的依赖传递



#### notes：如何创建bean对象？

1. 通过@Component及其衍生注解：@Controller、@Service、@Repository、@Mapper（这些注解是==针对项目中自己创建的bean对象使用的==）
2. 通过配置类中的@Bean方法：bean方法内部依然是通过new来创建对象（==配置类中的bean方法既可以用于项目中自己创建的bean对象使用也可以用于创建的第三方依赖中的bean对象==）
3. @Import注解：通过在==项目启动类上添加@Import注解==将第三方依赖中的bean对象添加到Spring容器中（==注解中声明的一般是配置类名，这样配置类中所有的bean方法创建的bean对象也被添加到了容器中，这个过程相当于一次性创建多个bean对象==，注解中也可以声明bean对象名直接将第三方依赖的bean对象添加到容器中但是用的少）（==@Import注解是专门用于创建第三方依赖中的bean对象的==，通过==该注解将第三方依赖中的bean对象添加到了容器中，后续要使用该bean对象的时候还是得通过@Autowired注解等方式从容器中获取bean对象==）

##### notes：配置类中的bean方法相较于@Component注解创建bean对象有什么区别？

配置类中的bean方法可以提供更精细的配置，比如当配置Interceptor拦截器时通过配置类中的bean方法可以配置拦截路径，而这仅仅通过@Component注解时无法实现的。



#### notes：如何从容器中获取bean对象？

1. 通过@Autowired注解：==既可以用于项目中自己创建的bean对象也可以用于第三方依赖中的bean对象==
2. 通过ApplicationContext.getBean方法：直接获取容器ApplicationContext，然后从容器中获取bean对象，不过这种方法用的少，==既可以用于项目中自己创建的bean对象也可以用于第三方依赖中的bean对象==

##### notes：关于从容器中获取bean对象的注意事项？

通过这两种方式获取bean对象时要求bean对象必须要在项目启动类所在的包及其子包下，如果不在的话得通过@ComponentScan注解标明bean对象所在的包，就比如说现在我们引入一个第三方的依赖，该依赖中已经创建了一个bean对象，该bean对象所在的包肯定不是当前项目启动类所在的包及其子包（它都是第三方依赖了，怎么还会在当前项目所在的包下呢？），这时候我们想要在当前项目中获取该bean对象的话肯定得通过@ComponentScan注解表明包扫描的路径，但是如果我们在当前项目中自己创建这个第三方bean对象的话（可以通过配置类中的bean方法或者是在当前项目的启动类上通过@Import注解将第三方依赖中的bean对象添加到容器中）就不需要再通过@ComponentScan注解了，因为==这时候第三方依赖中的bean对象就是在当前项目中创建的==（不管是当前项目中的配置类还是启动类它们所在的包都是启动类所在的包及其子包），==关键在于区分bean的创建和bean的来源这两个概念，第三方bean来源于第三方依赖但是它可以在当前项目中创建==





### （2）自动配置

#### 是什么：

SpringBoot项目中可以自动将第三方依赖中的bean对象添加到Spring容器中，不用我们在手动一个个去通过配置类中的bean方法或者是通过@Import注解一个个去添加

#### 为什么：

##### （1）为什么SpringBoot要使用自动配置的功能？

如果SpringBoot不具备自动配置的功能的话，那我们想要使用第三方依赖中的bean对象我们该怎么办？

方式1：第三方直接创建好bean对象，我们去引用

这时候我们可以通过@Autowired注解或者是ApplicationContext.getBean方法获取第三方的bean对象，但是得通过@ComponentScan注解额外注明第三方bean所在的包，当第三方bean很多时标注起来很麻烦

方式2：直接在当前项目中创建第三方的bean对象

这时候我们可以通过配置类中的bean方法或者是通过@Import注解将第三方依赖中的一个个bean对象添加到容器中后续再通过@Autowired注解等方式从容器中获取bean对象进行使用，当第三方bean很多时创建起来很麻烦

**发现不管是上述哪种方式都非常麻烦，需要一个个bean对象进行处理**，这就引入了SpringBoot的自动配置

#### 怎么办：

##### （1）SpringBoot怎么做到自动将第三方依赖中的bean对象添加到容器中的？

前面我们说过通过@Import注解（@Import注解中写的一般都是配置类名，从而将配置类中所有bean方法创建的bean对象添加到容器中）将第三方依赖的bean对象添加到容器中，但是一般情况而言我们并不清楚第三方依赖中有哪些配置只知道我们要使用的第三方依赖的bean对象是哪一个，该bean对象属于哪一个配置类我们并不清楚，所以我们无法在@Import注解中写明配置类，那该咋办？直接在@Import注解中写bean对象名？如果要使用很多个bean对象呢？一个个bean对象名的写？这也太麻烦了。

对于第三方依赖而言谁最清楚要使用的bean对象依赖于哪些配置类呢？当然是第三方依赖本身，所以第三方依赖自己就提供了一个@EnableXXXX开头的注解，该注解封装了@Import注解并且注解中已经写明了要使用的配置类并且这些配置类已经放在了默认位置下，所以我们只要在当前项目的启动类上添加@EnableXXXX注解就可以实现自动加载这些配置类从而将第三方依赖中的bean对象添加到容器中。

但是我们看一般的SpringBoot项目也没看到启动类上有什么@EnableXXXX注解啊，那为什么它依然可以使用第三方的bean对象呢？因为SpringBoot项目的@SpringBootApplication注解本身封装了3个注解：@SpringBootConfiguration、@ComponentScan、@EnableAutoConfiguration，其中在@EnableAutoConfiguration注解封装了@Import注解并且底层的方法中我们会去读取指定位置（`META-INF/spring.factories`和`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`）的配置文件，这些配置文件中写的都是要用到的配置类，通过加载这些配置类将第三方的bean对象添加到容器中，而这整个过程都是自动进行的，由此我们实现了自动配置

##### （2）配置类中的bean对象都会创建到容器中吗？

不是，通过@Conditional注解标明各种bean对象创建的条件



## 2、自定义starter

### 是什么：

我们将相关依赖以及bean对象的创建封装起来，后续要使用的时候直接引入该starter依赖即可，避免了再去手动创建bean对象的过程

### 怎么办：

#### （1）如何自定义starter？就拿文件上传到阿里云oss举例

1. 创建一个moduleA，该模块专门用来进行依赖管理（不写代码）
2. 创建一个moduleB，该模块进行自动配置，通过配置类创建bean对象，并将配置类名添加到spring.factory配置文件中（这样后续通过springboot的自动配置会自动去加载配置文件中写明的配置类）
3. 注明moduleA依赖moduleB，这样后续引入moduleA的时候会通过依赖传递自动引入moduleB

#### （2）自定义starter与普通的工具类模块之间有什么区别呢？

`starter`和普通的工具类模块之间最大的区别就在于`starter`中`bean`对象的创建是封装好直接添加到容器中的，而工具类模块中我们引入该依赖后还需要手动创建bean对（第三方本身没有创建bean对象，我们在容器中自己创建）或者是通过@Import注解将第三方依赖的bean对象添加到容器中（第三方创建了bean对象，我们将它添加到当前项目的容器中）

- 第三方本身没有创建bean对象，我们在容器中自己创建

![IMG_6585](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6585.JPG)

![IMG_6586](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6586.JPG)

![IMG_6587](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6587-1742311110144-3.JPG)



- 第三方创建了bean对象，我们将它添加到当前项目的容器中

![IMG_6588](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6588.JPG)

![IMG_6589](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6589.JPG)

- 通过自动配置实现自动创建第三方的bean对象

![IMG_6590](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6590.JPG)

![IMG_6591](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6591.JPG)

![IMG_6592](./SpringBoot%E5%8E%9F%E7%90%86%E7%AF%87.assets/IMG_6592.JPG)