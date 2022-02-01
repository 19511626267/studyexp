# JDBC入门

> JDBC本身就是一堆接口,但是经过数据库厂商的实现之后就可以连接并操作不同的数据库,**这样做的好处就是可以用相同的代码规范来操作不同的数据库,在项目的更改中如果要换数据库了也不用改大量的代码.非常的nice**



## 下面就让我们来接触一下JDBC的内容

> *JDBC中的类*
>
> 1. Driver类
>    1. 其实在写代码的时候用不到,他可以翻译成驱动器,用来驱动数据库,他是由数据库厂商来实现的一个类,可以说是内置啦
>    
> 2. DriverManager类 (以下的两个方法都是静态的)
>    1. 用于加载驱动器:(就是Driver) 这个方法叫registerDriver();
>
>       - 我们一般不用DriverManager.registerDriver(Driver driver)的方法来加载驱动因为Driver类 中由静态代码块自动加载驱动,在用registerDriver(Driver driver)的话就实例化了两个Driver对象 所以我们一般用***Class.forName("com.mysql.jdbc.Driver");***就直接加载了Driver类  (forName:Class.forName(xxx.xx.xx);的作用是要求JVM查找并加载指定的类，也就是说JVM会执行该类的静态代码段)
>
>    2. 用于和数据库建立链接: getConnection( String url , String user , String pwd ); 
>
>       - 该方法会返回一个Connection对象,这个对象代表对这个程序与这个数据库的这个链接 
>
>       - **url**的格式是jdbc:mysql://hostname:port/datebasename;  
>         - 其中hostname代表数据库所在的ip地址在本机上可以写127.0.0.1或者localhost如果在其他机器上写他的ip
>          - port代表端口号 mysql的默认值是3306
>          - databasename是数据库名称
>       - user是用户名 pwd是密码
>
> 3. Connection类 只有获得了这个类后才能访问数据库
>
>    1. creatStatement() 用于返回一个statement类 这个类能对数据库进行访问
>    2. prepareStatement(String sql) 用于返回一个PreparedStatement类 这个类也是对数控进行访问的马上我们就讲到了
>    3. prepareCall()用于返回一个CallableStatement他对象 可以调用数据库中的存储过程,下面是解释但是没怎么看懂
>       - *存储过程是一个预编译的SQL语句，比如一些场景的sql比较复杂，并且需要经常使用或者多次使用的。存储过程的优点是说只需创建一次编译一次，以后在该程序中就可以多次直接调用。如果一个sql是经常需要操作的，并且逻辑不容易改变，使用存储过程比单纯SQL语句执行要快，因为sql每次查询而且都需要编译。而且网络开销也大，存储过程只需要传一个名字，在数据库调用就行了，而且这样程序可移植高
>
> 4. Statement类
>
>    1. execute( String sql) 
>       - 用于执行sql语句 String值就是语句 返回bool值如果为true就是用查询结果,可以通过Statement的getResultSet()方法获得查询结果;
>    2. executeUpdate(String sql); 
>       - 用于执行sql中的insert update delete语句返回 int 值 代表受该sql语句影响的记录条数
>    3. executeQuery(String sql)
>       - 用于执行select语句返回一个ResultSet对象
>
> 5. PreparedStatement类 (相较于Statement类,语句更加灵活,因为Statement类中方法都是直接传入一个语句所以操作比较局限,必须严格规定语句中各个的数据,不利于批量操作,但是PreparedStatement正好可以弥补缺点)
>
>    <先用>prepareStatement(String sql)返回一个PreparedStatement类 其中的sql语句可以设置成范式的形式用占位符?代替代表这个PreparedStatement类执行的sql语句
>
>    1. executeUpdate()
>       - 执行这个PreparedStatement类的sql语句
>    2. setInt(int parameter,int x)
>       - 为语句的指定参数位置设定指定的int值
>    3. setFloat(int parameter,float x)
>       - 为语句的指定参数位置设定指定的float值
>    4. setString(int parameter,String x)
>       - 为指定位置的参数设置指定的string值
>    5. setData(int parameter,Data x)
>       - 为指定位置的蚕食设定指定的Data值
>    6. executeQuery()
>       - 执行sql查询返回ResultSet对象
>    7. 

