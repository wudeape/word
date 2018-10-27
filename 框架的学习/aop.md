   # aop  的认识
   AOP技术恰恰相反，它利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性
  1核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来


  ## spring对aop的支持
   Spring中AOP代理由Spring的IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理。因此，AOP代理可以直接使用容器中的其它bean实例作为目标，这种关系可由IOC容器的依赖注入提供。Spring创建代理的规则为：

1、默认使用Java动态代理来创建AOP代理，这样就可以为任何接口实例创建代理了

2、当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理，也可强制使用CGLIB

AOP编程其实是很简单的事情，纵观AOP编程，程序员只需要参与三个部分：

1、定义普通业务组件

2、定义切入点，一个切入点可能横切多个业务组件

3、定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

依赖注入和控制反转
  依赖注入 上面将依赖在构造函数中直接初始化是一种 Hard init方式，弊端在于两个类不够独立，不方便测试。我们还有另外一种 Init 方式
     public class Human {
    ...
    Father father;
    ...
    public Human(Father father) {
        this.father = father;
    }
}
像这种非自己主动初始化依赖，而通过外部来传入依赖的方式，我们就称为依赖注入

public class Human {
    ...
    @Inject Father father;
    ...
    public Human() {
    }
}
   ### 关于很好理解依赖注入的例子 （https://www.cnblogs.com/xiaoxi/p/5935009.html）
也是一种依赖注入的方式
 ## 关于spring的依赖注入可能出现的问题
   使用spring注解功能也可以实现自动注册dao和bean的功能 之前是在配置文件中（xml文件中）
 example:
 配置文件是
 <bean id="UserVODao" class="com.xinyiglass.springSample.dao.impl.UserVODaoImpl" parent="abstractDao"/>
<bean id="abstractDao" abstract="true"><property name="dataSource" ref="dataSource"/></bean>
在添加注解后
方法1：最简洁，用重写类构造函数的办法自动装配daoSupport需要用到的数据源
@Autowired
    UserVODaoImpl(DataSource dataSource) {
        setDataSource(dataSource);
    }
    方法2：也是差不多，初始化的时候自动装配数据源
      @Autowired
    private DataSource dataSource;

    @PostConstruct
    private void initialize() {
       setDataSource(dataSource);
    }

 ## spring的一些注解 (http://www.jb51.net/article/75460.htm)
1 @ Autowired 
@Autowired可以对成员变量、方法和构造函数进行标注，来完成自动装配的工作，，@Autowired的标注位置不同，
它们都会在Spring在初始化userManagerImpl这个bean时，自动装配userDao这个属性，区别是：第一种实现中，
Spring会直接将UserDao类型的唯一一个bean赋值给userDao这个成员变量；第二种实现中，
Spring会调用setUserDao方法来将UserDao类型的唯一一个bean装配到userDao这个属性。
 
2 @Resource  （一般来代替@Autowired）
@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource默认按byName自动注入罢了
        public class Zoo1 {
    @Resource(name="tiger")
    private Tiger tiger;

   @Resource(type=Monkey.class)
   private Monkey monkey;
    
   public String toString(){
  return tiger + "\n" + monkey;
  }
}
