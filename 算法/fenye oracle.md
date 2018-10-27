String s_pageNow! = (String)request.getParamet4er("PageNow");
 int pageNow =1;
  if(s_pageNow!= null){
   pageNow =Integer.parseInt(s_pageNow);
  }    //强制类型转换

  int pageCount =0;
  int rowCount=0;  // 共有几条记录
  int pageSize =3 ; 每页显示几条记录

  ResultSet rs = sm.executeQuery("select  count(*) from emp");
  // 获得所有的结果集

  if (rs.next()){
 rowCount = rs.getInt(1);
 if(rowCount %pageSize ==0){
   pageCount = rowCount /pageSize;
 } else{
 pageCount = rowCount/ pageSize+1;
 }

  }
 rs=sm.executeQuery("select * from (select a1.*,rownum rn from (select * from emp) a1 where rownum<="+pageNow*pageSize+") where rn>="+((pageNow-1)*pageSize+1)+"");
 // 就是所谓的分页查询
 while(rs.next()){
  out.println("<tr>");
			//用户名
			out.println("<td>"+rs.getString(2)+"</td>");
			//密码
			out.println("<td>"+rs.getString(6)+"</td>");
			out.println("</tr>");
 }
// 打印超链接
for ( int i=1;i<=pageCount ;i++){
	out.print(" <a href = jsp ?pageNow = "+i+"> ["+i+"] </a>")
}