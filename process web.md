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

# 登录部分的实现（功能是不完全的）
+ dao部分是进行数据的处理部分需要进行与数据库的交互 目前是login,isExist,save(register)，isExistResume..
+ servlet 是来处理逻辑部分 
*  registersevelet 部分 
request.getParameter 来获取email和password 然后 对email if  else 两种不同结果的实现
*  loginsevelet 部分
（首先是js的校检）onsubmit  然后email 和password的比较 目前还没有实现相应的登录验证 等功能

#  个人简历
* 数据的存入部分（dao部分) 以下是servlet部分
 String type = request.getParameter("type");
 if ("add".equals(type)) {
			ResumeBasicinfo basicinfo = this.requestDataObj(request);		
			ResumeDAO dao = new ResumeDAO();
		
   int basicinfoID = dao.add(basicinfo, 1);
		
response.sendRedirect("applicant/resume.html");
}
 
 与前端页面对应的是 /Q_ITOffer_Chapter03/ResumeBasicinfoServlet?type=add  全路径

 * 如果进行数据更新 resumemessage 
 将简历信息封装成一个对象 本体的例子是在servlet封装，
 首先是获取相应页面的信息
 request.getParameter("");  存在string中
 basicinfo = new ResumeBasicinfo(realName, gender, birthdayDate,
				currentLoca, residentLoca, telephone, email, jobIntension,
				jobExperience);
return basicinfo;

# 图片的上传






 