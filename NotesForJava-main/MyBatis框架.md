## MyBatis

### #{}和${}的区别

${}:是Properties文件中的变量占位符，可以用于标签属性值和SQL内部，属于静态文本替换，比如${driver}会被静态替换为com.mysql.jdbc.Driver

#{}:是SQL参数占位符，MyBatis会将SQL中的#{}替换为?，在SQL执行前会使用PreparedStatement的参数设置方法，按序给SQL的?占位符设置参数

### XML映射文件中，除了常见的select|insert|update|delete标签外，还有哪些标签

resultMap, parameterMap, sql, include, selectKey，加上动态SQL的9个标签，trim, where, set, foreach, if, choose, when, otherwise, bind，通过include标签引入sql片段，selectKey为不支持自增的主键生成策略标签

### 通常一个XML映射文件，都会有DAO接口与之对应，相关DAO知识

DAO接口，就是人们常说的Mapper接口，接口的全限名，就是映射文件中的namespace的值，接口的方法名，就是映射文件中MappedStatement的id值，接口方法内的参数，就是传递给sql的参数。

Mapper接口没有实现类，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MappedStatement。在MyBatis中，每一个select， insert，update，delete标签都会被解析为一个MappedStatement对象

DAO接口里的方法不能重载，因为是全限名+方法名的保存和寻找策略

DAO接口的工作原理是JDK动态代理，MyBatis运行时会使用JDK动态代理为DAO接口生成代理proxy对象，代理对象proxy会拦截接口方法，转而执行MappedStatement代表的SQL，然后将执行结果返回。

### MyBatis的插件

#### 如何进行分页，分页插件的工作原理

MyBatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页，可以在SQL内直接书写带有物理分页的参数来完成物理分页，也可以使用分页插件来完成物理分页。

分页插件的基本原理是是使用MyBatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的SQL，然后重写SQL，根据dialect方言，添加对应的物理分页语句和物理分页参数

#### MyBatis插件运行原理以及如何编写插件

MyBatis仅可以编写针对`ParameterHandler`,`ResultSetHandler`,`StatementHandler`,`Executor`四种接口的插件，MyBatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这四种接口对象的方法时，就会进入拦截方法，具体就是`InvocationHandler`的`invoke()`方法。

实现MyBatis的Interceptor接口并复写intercept()方法，然后再给插件编写注解，指定要拦截哪一个接口的哪些方法即可，同时在配置文件中配置编写的插件。

### MyBatis的动态SQL

MyBatis动态SQL可以让我们在XML映射文件内，以标签的形式编写SQL，完成逻辑判断和动态拼接SQL的功能。MyBatis的9种动态SQL标签

trim, where, set, foreach, if, choose, when, otherwise, bind

执行原理：使用OGNL从SQL参数对象中计算表达式的值，根据表达式的值拼接SQL，以此来完成动态SQL的功能

### MyBatis如何将SQL执行结果封装为目标对象并返回

1. 使用resultMap标签，逐一定义列名和对象属性名之间的映射关系。
2. 使用SQL列的别名功能，将列别名书写为对象属性名

有了列名与属性名的映射关系后，MyBatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性是无法完成赋值的。

### MyBatis的一对一，一对多关联查询的实现

关联对象查询有两种方式

1. 单独发送一个SQL去查询关联对象，赋给主对象，然后返回主对象。
2. 使用嵌套查询，使用join，一部分列是A对象的属性值，另一部分是关联对象B的属性值，好处是只发一个SQL就可以把主对象和关联对象查出来

join查询出来如何去重

resultMap标签内的id子标签，制定了唯一确定一条记录的id列，MyBatis根据列值来完成去重功能。

### MyBatis的延迟加载

MyBatis仅支持association关联对象(一对一)和collection关联集合对象(一对多)的延迟加载

在MyBatis配置文件中，可以配置是否启用延迟加载`lazyLoadingEnabled=true/false`

原理：

使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法(例如`a.getB().getName()`)，拦截器`invoke()`方法发现`a.getB()`是null，那么就会单独发送事先保存好的查询关联B对象的SQL把B查询上来，然后调用`a.setB(b)`，接着完成`a.getB().getName()`方法的调用。

### MyBatis中不同XML文件，id是否可以重复

如果不同的XML文件配置了namespace，那么id可以重复

如果不同的XML文件没配置namespace，那么id不可重复

因为namespace+id是作为Map<String, MappedStatement>的key来使用的，如果没有namespace，只剩下id，那么id重复就会被覆盖。

### MyBatis的Executor执行器

MyBatis的三种处理器：`SimpleExecutor`,`ReuseExecutor`,`BatchExecutor`

SimpleExecutor：每执行一次update或者select，就开始一个Statement对象，用完理科关闭这个Statement对象

ReuseExecutor：执行update或select，以SQL作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，存放在Map<String, MappedStatement>内，以便下一次使用

BatchExecutor：用来完成批处理。执行update（没有select，因为JDBC批处理不支持select），将所有的SQL都添加到批处理中(`addBatch()`)，等待统一执行(`executeBatch()`)，它缓存了多个Statement对象。与JDBC的批处理相同

#### 如何制定某种处理器

在MyBatis配置文件中，可以制定默认的ExecutorType执行器类型，也可以手动给DefaultSqlSessionFactory的创建SqlSession方法川力ExecutorType类型参数

### MyBatis是否可以映射Enum枚举类

可以。

除了Enum类，还可以映射任何对象到表的一列上。映射方式为自定义一个TypeHandler，实现TypeHandler的setParamter()和getResult()接口方法。

TypeHandler的两个作用

1. 完成从javaType到jdbcType的转换
2. jdbcType到javaType的转换。对应上述两个方法

### 如果A标签include引用了B标签内容，B该如何定义

虽然MyBatis解析XML映射文件是按照顺序解析的，但是被引用的B标签依然可以定义在任何地方，MyBatis都可以正确识别

原理：

MyBatis解析到A标签，发现引用了B，此时B还没解析到，尚不存在，则将A标签定义为未解析状态，开始解析其他标签，等全部标签解析完，再去解析那些标记为未解析状态的标签，此时再解析到A的时候，B已经解析完了，所以A可以正常完成解析

### MyBatis的XML映射文件和MyBatis内部数据结构之间的映射关系

MyBatis将所有的XML配置信息都封装到All-In-One重量级对象Configuration内部。在XML映射文件中，<paramterMap>标签被解析为ParameterMap对象，其每个子元素被解析为ParameterMapping对象。<resultMap>标签解析为ResultMap对象，子元素解析为ResultMapping对象。每一个<select>,<insert>,<update>,<delete>标签均会被解析为MappedStatement对象，标签内的SQL会被解析为BoundSql对象

### MyBatis的半自动ORM映射工具与全自动的区别

Hibernate属于全自动ORM映射工具，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。

MyBatis在查询关联对象或者关联集合对象时，需要手动编写SQL来完成，所以是半自动ORM映射工具。

