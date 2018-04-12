# 常用工具
## stirng 和 stringBuffer    
//  
1 string 类（final） 是不可继承的
* 三种比较方式 （s1 == s2）;( s1.equals(s2)) ; 
int x = s1.compareTo (s2);  返回三，大于等于小于  字符的大小写是有区别的
* 去串的子串 和指定位置的串
substirng (int start)
substring ( start end)  是start值 到end-1 
indexOf() 查找 ，从左到右 返回的是第一个字母所在的位置

2 stringBuffer类 （内容是可以修改的）
自动开辟存储空间 buf.capacity() 获取当前的容量 buf.length() 长度
 追加对象  buf.append().append()....
 插入子串  buf.insert (int index,Object);  第index+1 的位置插入object
 删除子串 delete (int start, int end) 删除第start+1 到end 的串

## 日历类的使用
1 Date 和 DateFormat 的使用
java.util.Date  获取系统日期
 Date date = new Date();    获取日期构造的对象
 int year = date.getYear()+ 1900;   // 当前年份与1900 之间的时间差
 int month = date.getMonth() +1;    //从0开始
 int hour= date.getHours();
 int minuters =date.getMinutes();
构造函数内部使用的是  System.currentTimeMills() 方法    //数据库存储的时间 自动获取

2 日期格式化
java.text.SimpleDateFormat 和它的抽象类 java.text.DateFormat;
对输出时间的一种控制 几种常用的输出格式如下
SimpleDateFormat format = new SimpleDateFormat ("yyyy年MM月dd日HH时mm分ss)；
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
SimpleDateFormat format = new SimpleDateFormat("yy/MM/dd  HH:mm");
SimpleDateFormat format = new SimpleDateFormat
计算日期差 Date() 类 提供了getTIme() 函数计算两个日期之间的差的天数等

Date date = new Date();
long l = date.getTime();
Date dl  = new Date();
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
String s = " 1980-05-01";
Date d2 = format.parse(s);    // format.parse  是把文本数据解析成日期对象
int days = (int)( d1.getTime() - d2.getTime()) / (1000*60*60。);

3 Calendar 日历类的使用
一般是用来进行日历的一些计算 
Calendar cal = new Calendar.getInstace();
int  day = cal.get(Calendar.DAY_OF_YEAR);
int week = cal.get(Calendar.YEAR);
int days = cal.get(Calendar.)






## EL 和 JSTL 的使用
El 便于存取数据 减少脚本与html 的嵌套
1 输出 某一个范围的变量值 
<% = session.getAttribute("name")%>
or 
$ { name} or $ {sessionScope.name}

2 输出页面之间传的值
request.getParameter(String name);
request.getParameterValues (String name)

or 
$ {param.name}
$ {paramValues.name}

3获取cookie 的值
 for example
 <%  
   Cookie c = new Cookie("uemail","1497595896@qq.com");
   response.addCoodie(c);
 %> 
 <input type ="text" value="${cookie.uemail.value}">

EL 输出javaBean中的属性值			
<jsp:useBean id = "user" class = "bean.User"></jsp:useBean>
<%
  user.setUname("zhou");
%>
 name : ${user.uname}

输出集合元素   ${集合名称[索引]}
<%
   ArrayList list = new ArrayList();
   list.add("a");
   list.add("b");
   list.add("c");
   pageContext.setAttribute("arr",list);
   ArrayList<User> list2 =new ArrayList<User>();
   	User user = new User();
   	user.setUname("zhou");
   	user.setUpawd("1234");
    pageContext.setAttribute("clist",list2);
%>
 ${arr[0],$arr[1],$arr[2]<br>}
 ${clist[0].uname}

 JSTL标签的使用
 核心库的声明
 <%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core" %>
 sql库声明 
 <%@ taglib prefix="sql"   uri="http://java.sun.com/jsp/jstl/sql" %>
 xml库声明
 <%@ taglib prefix="xml"  uri="http://java.sun.com/jsp/jstl/xml" %>
 函数标签库声明
 <%@ taglib prefix="fn"  uri="http://java.sun.com/jsp/jstl/fn" %>

核心库  基本的输入输出 流程控制 迭代操作 URL操作
<c:out> 输出一定范围的属性 相当于jsp 的out.pritln();  <c:out value = "hello world" // vaule = ${use.uname}>
<c:set> 设定某个对象的属性 
<c:set vaule = "vaule" var = "varName">  [scope ="page|request|session|application"]
例如 <c:set vaule ="zhou" var ="uname/">

１.scope的作用域大小依次为：application>session>request>page(默认)
２.jsp处理变量的作用域先后依次为：page(默认)->request->session->application

相当于
<%
pageContext.setAtrribute("uname","zhou");
%>

<c:set vaule ="zhou" var ="uname" scope="session"/>


3  删除某个变量或者是属性  <c:remove var ="varName"> [scope ="page|request|session|application"]
<c:remove vaule ="zhou" var ="uname" scope="session"/>

4 为bean属性赋值
<jsp:useBean id="user" class ="zhou.User"></jsp:useBean>
<c:set target="${user}" property ="uname" vault="admin"/>


流程控制  catch ,if , choose ,when, otherwise
<c:catch var ="">  此处的var相当于一个id   </c:catch>  就是用来捕捉异常的
相当于jsp中的 <% try{} catch{}%>

<c:if> 就是一种选择 输出 </c:if>

<body>
	<c:set var ="score" vaule ="81">
	<c:if test="${score > 90}">
	成绩优异
</c:if>
   <c:if test="${score<80}">
    <c:out vaule="">
   </c:if>
</body>

最常用的对数据库的操作 迭代操作 forEach ,forTokens
forEach 相当于jsp的 for(int i=j;i<k;i++)
  基本的语法 
<c:forEach  [var ="varName"] item="collection" [varStatus="varStatus"] [begin ="begin"] [end="end"][step="step"]>
body 内容
</c:forEach>
 简单的遍历输出
<body>
	//固定次数循环
 	<c:forEach var ="count" begin ="50" end ="60">
 	<c:out vaule ="${count}"/>
 </c:forEach>
    <%
     Arraylist<User> users = new ArrayList<User>();
     	for (int i=1;i<4;i++){
     	User user = new User();
     	user.setUname("zhou");
     	user.setUpwd("1234");
     	pageContext.setAtrribute("userlist",users);   //  此处的userlist相当于属性 users相当于值
     	//遍历集合的内容
     	<c:forEach var ="user" item ="${userlist}">
     	<tr>
     		<td> <c:out vaule="${user.name}"/></td>
     		<td><c:out vaule ="${user.upwd}"/></td>
   </tr>
   }
   %>
 </body> 

 URL标签的使用
 <c:import url=""> 的作用就是引入一个url资源
 相当于 jsp的<jsp:include page="">
 <c:redirect url="">
 相当于jsp的 request.sendRedirect("")

 jstl 输出数据库的表内容
 

