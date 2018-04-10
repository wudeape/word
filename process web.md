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
+  <input name="headShot" id="headShot" type="file" value="上传照片"> 然后在头部的js 进行图片的校检  type= file
<script  type="text/javascript" charset="utf-8" >
	function validate(){
			var headShot = document.getElementById("headShot");
		if (headShot.value == "") {
			alert("请选择要上传的头像！");
			headShot.focus();
			return false;
		}
		return true;
	}
</script>

同样的文件的上传是放在 form表单中
<form action="/Q_ITOffer_Chapter03/ResumePicUploadServlet" method="post">
	接收响应后 servlet 处理   实际实现文件上传的是servlet的注解 @MultipartConfig  (其他的框架是使用各自封装的方法类)
首先是获取上传文件域
Part part = request.getPart("headShot");   就是表单的class
// 获取上上传文件的名字
String Filename = part.getSubmittedFilename();
//防止文件重名，进行重定义
String newFilename = System.currnetTimeMills()+fileName.substirng(fileName.lastIndexOf("."));
//文件的保存路径
String filepath= getServletContext().getRealPath("url");
System.out.println("头像保存路径为：" + filepath);
		File f = new File(filepath);
		if (!f.exists())
			f.mkdirs();
		part.write(filepath + "/" + newFileName);
// 更新简历照片
ResumeDAO dao = new ResumeDAO();
dao.updateHeadShot(1,newFileName)；   // 序列是不对的 ？ 怎样合理的存放
response.sendRedirect ("url");  // 一般是默认重定向在本页面


public void updateHeadShot(int basicinfoId, String newFileName) {
		String sql = "UPDATE tb_resume_basicinfo SET head_shot=? WHERE basicinfo_id=?";
		Connection conn = DBUtil.getConnection();
		PreparedStatement pstmt = null;
		try {
			pstmt = conn.prepareStatement(sql);
			pstmt.setString(1, newFileName);
			pstmt.setInt(2, basicinfoId);
			pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			DBUtil.closeJDBC(null, pstmt, conn);
		}
}


# 注册验证码的实现
<div class="span1">
<label class="tn-form-label">验证码：</label> <input
		class="tn-textbox-long" type="text" name="verifyCode"> <span>
		<img src="ValidateCodeServlet"  // 此处相应后台servlet生成过程
	id="validateCode" title="点击换一换" onclick="changeValidateCode()">
	<a href="javascript:changeValidateCode();">看不清？</a>
 </span>
</div>
  
 验证码的更换 
 function changeValidateCode() {
document.getElementById("validateCode").src = "ValidateCodeServlet?rand="
	+ Math.random();
}

js实现验证码（手机号验证 未完成）
此处是   只是前台的页面进行验证的输入
<script type="text/javascript">
var code;
function createCode() 
{
 code = "";
 var codeLength = 6; //验证码的长度
 var checkCode = document.getElementById("checkCode");
 var codeChars = new Array(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 
      'a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z',
      'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'); //所有候选组成验证码的字符，当然也可以用中文的
 for(var i = 0; i < codeLength; i++) 
 {
  var charNum = Math.floor(Math.random() * 52);
  code += codeChars[charNum];
 }
 if(checkCode) 
 {
  checkCode.className = "code";
  checkCode.innerHTML = code;
 }
}
function validateCode() 
{
 var inputCode=document.getElementById("inputCode").value;
 if(inputCode.length <= 0) 
 {
  alert("请输入验证码！");
 }
 else if(inputCode.toUpperCase() != code.toUpperCase()) 
 {
   alert("验证码输入有误！");
   createCode();
 }
 else 
 {
  alert("验证码正确！");
 }    
}  
</script> 


## 回话跟踪部分
# 将验证码保存到session中
request.getSession().setAttribute("SESSION_VALIDATECODE", code);
 code 是servlet部分生成的验证码，存储在session的作用是  
 在进行注册时 验证码作为首先校检 然后进行其他条件的判断
 String email = request.getParameter("email");
		String password = request.getParameter("password");
		String verifyCode = request.getParameter("verifyCode");
		// 判断验证码是否正确
		String sessionValidateCode = (String)request.getSession().getAttribute("SESSION_VALIDATECODE");
		if(!sessionValidateCode.equals(verifyCode))      
		{
		out.print("<script type='text/javascript'>");                                                        
		 out.print("alert('请正确输入验证码！');");
			out.print("window.location='register.html';");
			out.print("</script>");
		}else{
			// 判断邮箱是否已被注册
			ApplicantDAO dao = new ApplicantDAO();
			boolean flag = dao.isExistEmail(email);
			if(flag){
				// 邮箱已注册,进行错误提示
				out.print("<script type='text/javascript'>");                                                
				out.print("alert('邮箱已被注册，请重新输入！');");
				out.print("window.location='register.html';");
				out.print("</script>");
			}else{
				// 邮箱未被注册，保存注册用户信息
				dao.save(email,password);
				// 注册成功，重定向到登录页面
				response.sendRedirect("login.html");
			}
		}
	}

# 完善登录功能

在用户登录成功后进行对求职者的信息跟踪，检验是否有简历，若有的话进行简历信息的跟踪
 添加Applicat.java  进行对求职者信息的封装 
  if (applicantID != 0) {
// 用户登录成功,将求职者信息存入session      
 Applicant applicant = new Applicant(applicantID, email, password);
			request.getSession().setAttribute("SESSION_APPLICANT", applicant);
			// 通过Cookie记住邮箱和密码
			rememberMe(rememberMe, email, password, request, response);
			// 判断是否已填写简历
			int resumeID = dao.isExistResume(applicantID);           
			if (resumeID != 0){
				request.getSession().setAttribute("SESSION_RESUMEID", resumeID);
				// 若简历已存在，跳到首页
				response.sendRedirect("index.jsp");
			}else
				// 若简历不存在，跳到简历填写向导页面
				response.sendRedirect("applicant/resumeGuide.html");
		} else {
			// 用户登录信息错误，进行错误提示
			out.print("<script type='text/javascript'>");                                                    
			out.print("alert('用户名或密码错误，请重新输入！');");
			out.print("window.location='login.html';");
			out.print("</script>");
		}
	}

简历信息添加和图片的上传在操纵数据前都先从回话中获取用户的标识符（简历标识符），添加部分如上
  // 简历添加操作
		if ("add".equals(type)) {
			// 封装请求数据
			ResumeBasicinfo basicinfo = this.requestDataObj(request);
			// 从会话对象中获取当前登录用户标识
			Applicant applicant = (Applicant)request.getSession().getAttribute("SESSION_APPLICANT");
			// 向数据库中添加当前用户的简历
			ResumeDAO dao = new ResumeDAO();
			int basicinfoID = dao.add(basicinfo, applicant.getApplicantId());
			// 将简历标识存入会话对象中
			request.getSession().setAttribute("SESSION_RESUMEID", basicinfoID);
			// 操作成功，跳回“我的简历”页面
			response.sendRedirect("applicant/resume.html");
		}

// 从会话对象中获取当前用户简历标识
		int resumeID = (Integer)request.getSession().getAttribute("SESSION_RESUMEID");
		// 更新简历照片
		ResumeDAO dao = new ResumeDAO();
		dao.updateHeadShot(resumeID, newFileName);
		// 照片更新成功，回到“我的简历”页面
		response.sendRedirect("applicant/resume.html");


# 使用cookie记住登录信息 (并且获取)
+  首先是jsp中的集成，java中读取cookie的信息
cookie 的使用
<%
String applicantEmail = "";
String applicantPwd = "";
// 从客户端读取Cookie     都是遍历获取客户端的cookis
Cookie[] cookies = request.getCookies();  
if (cookies != null) {  
  for (Cookie cookie : cookies) { 
  加密处理 
    if ("COOKIE_APPLICANTEMAIL".equals(cookie.getName())) {  
    	// 解密获取存储在Cookie中的求职者Email
      applicantEmail = com.qst.itoffer.util.CookieEncryptTool.decodeBase64(cookie.getValue());   
    }  
    if ("COOKIE_APPLICANTPWD".equals(cookie.getName())) {  
    	// 解密获取存储在Cookie中的求职者登录密码
      applicantPwd = com.qst.itoffer.util.CookieEncryptTool.decodeBase64(cookie.getValue());  
    }  
  }
}
%>

jsp 页面页面文件中设置表单的  value  其作用是 读取<% %> 获取的cookie值
 value="<%=applicantEmail%
  value="<%=applicantPwd%   应该有一个触发或者按钮来实现 值的一次性获取
 rememberMe 功能 就是一个选择框
 <input	checked="checked" name="rememberMe" id="rememberMe"	class="tn-checkbox" type="checkbox" value="true"> <label for="RememberPassword" style="color: gray"> 记住密码</label>
具体的实现在servlet 中
// 通过Cookie记住邮箱和密码
...    设置email，passowrd，设置参数获取
			rememberMe(rememberMe, email, password, request, response);
	rememberMe() 的实现方法 （cookie 的具体使用）  session在前，cookie在后，登录成功后，if  删除cookie else  保存，跳转 简历部分

if ("true".equals(rememberMe)) {
			// 记住邮箱及密码
// cookie 的创建 Cookie lalacookie = new Cookie("username","parm");
			Cookie cookie = new Cookie("COOKIE_APPLICANTEMAIL",
					CookieEncryptTool.encodeBase64(email));
			cookie.setPath("/");
			cookie.setMaxAge(365 * 24 * 3600);
			response.addCookie(cookie);

cookie = new Cookie("COOKIE_APPLICANTPWD",
					CookieEncryptTool.encodeBase64(password));
			cookie.setPath("/");
//  cookie 的存活时间 setMaxAge 的值是 0 表示删除，负数是临时
			cookie.setMaxAge(365 * 24 * 3600);
			response.addCookie(cookie);
		} else {
			// 将邮箱及密码Cookie清空
			Cookie[] cookies = request.getCookies();
			if (cookies != null) {
				for (Cookie cookie : cookies) {
					if ("COOKIE_APPLICANTEMAIL".equals(cookie.getName())
							|| "COOKIE_APPLICANTPWD".equals(cookie.getName())) {
						cookie.setMaxAge(0);
						cookie.setPath("/");
						response.addCookie(cookie);
					}
				}

本项目中添加了cookieEnvryToo.java 进行加密和解密
一般的是采用md5 （待实现）  本次项目是base64
加密   （就是实现类型的转换，安全性较差）
<pre>
	try {
			cleartext = new String(Base64.encodeBase64(cleartext
					.getBytes("UTF-8")));
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
		return cleartext;
</pre>
解密
<pre>
		try {
			ciphertext = new String(Base64.decodeBase64(ciphertext.getBytes()),
					"UTF-8");
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
		return ciphertext;
	}
</pre>


    
## 补充 File （java中流）
  + 待补充 （嘿嘿）
+ 借此分析一下dao ,一般都是 先进行 sql语句，然后 数据库创建连接 conn，pstmt, try catch进行在数据库的操纵，pstmt.set()  方法实现



 