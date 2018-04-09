## 贯穿web项目中的一些实现
# 所有的实现是基于servlet
 *  重定向和请求转发 
     具体的实现时都是在doget方法中，是属于从页面中获取请求
 + 重定向 ： 进行两次请求和响应（是servlet请求转变）
<pre>
	<!--  是页面的重定向-->
	response.sendRedirect("/url/jsp");
	<!--  是servlet的重定向，注意获取当前地址的方法-->
	response.sendRedirect(request.getContextPath()+"/otherservelt");
</pre>
 + 请求转发 是服务器端的servlet变化 对应的地址栏并不会发生变化
  使用的是requestDispatcher 中的forward() 方法
  <pre>
  	RequestDispatcher dispatcher = request.getRequestDispatcher("/Resultservlet");
  	dispatcher.forwoard( request, response);
  </pre>
 
  + servlet 数据处理部分
  * 超链接形式获取  
  <a href= "url地址?参数 =参数值 [&参数 = 参数值...]"> </a>
  example
  <a href="LinkRequestServlet?pageNo=2&queryString= QST"> </a>
  about the servlet section
  .........
  ....
  String pageNo = request.getParameter("pageNO");
  String queryString = request.getParameter("queryString");
  int pageNum =0;
  if(pageNo!=NULL)
  pageNum = Integer.parseInt(pageNo);

  * form 表单action进行获取
  注意 的是怎样在servlet中进行数据的捕捉和输出(肯定需要获取相应的对象)
  PrintWrite out = response.getWriter();   // 相应输出对象

# task1 需要注意的一些实现
 * 正则表达式  http://tool.oschina.net/uploads/apidocs/jquery/regexp.html 
 一些常用的正则表达式  （用来校检客户端的规格）
+ 用户名 	/^[a-z0-9_-]{3,16}$/
+ 密码  	/^[a-z0-9_-]{6,18}$/
+ 十六进制  /^#?([a-f0-9]{6}|[a-f0-9]{3})$/
+ URL /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
+ 电子邮箱  /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
/^[a-z\d]+(\.[a-z\d]+)*@([\da-z](-[\da-z])?)+(\.{1,2}[a-z]+)+$/
+ ip地址    /((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)/
/^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/
+ HTML标签  	/^<([a-z]+)([^<]+)*(?:>(.*)<\/\1>|\s+\/>)$/

# 一般的js校检都是在<head></head> 部分中  
<<script  type="text/javascript">
	Function validate(){
 var lal...

 Function showdiv(){
 	document.getElementById()
 }
	
</script>
 具体的在jsp中form表单中添加触发事件 
 <form action="" method="get" onsubmit="return validate();">
 </form>

另一种方式 
<a href="javascript:showdiv();" "email me">email me</a>




 