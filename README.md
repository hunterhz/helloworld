# helloworld
package com.bjpowernode.oa.web.action;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;



public class DeptListServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		PrintWriter out =response.getWriter();
		
		out.print("<!DOCTYPE html>");
		out.print("<html>");
		out.print("<head lang='en'>");
		out.print("    <meta charset='UTF-8'>");
		out.print("    <title></title>");
		out.print("</head>");
		out.print("<body>");
		out.print("<a href='/servlet-04/add.html'>add dept</a>");
		out.print("<hr>");
		out.print("<h2 align='center'>DEPT LIST</h2>");
		out.print("<hr>");
		out.print("<table border='1px' width='70%' align='center'>");
		out.print("    <tr>");
		out.print("        <th>DEPTNO</th>");
		out.print("        <th>DNAME</th>");
		out.print("        <th width='30%'>OPERATE</th>");
		out.print("    </tr>");
		
		
		//out.print("  hahahahahhahahaha");
		
		
		Connection conn =null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		
		try {
			//注册驱动
			Class.forName("com.mysql.jdbc.Driver");
			//获取连接
			String url ="jdbc:mysql://localhost:3306/bjpowernode";
			String user = "root";
			String password = "zpj0428";
			conn = DriverManager.getConnection(url,user,password);
			//获取预编译的数据库操作对象
			String sql = "select deptno , dname from dept";
			ps = conn.prepareStatement(sql);
			//执行SQL语句
			rs = ps.executeQuery();
			//处理查询结果集
			while (rs.next()) {
				String deptno = rs.getString("deptno");
				out.print("    <tr align='center'>");
				out.print("        <td>"+ deptno +"</td>");
				out.print("        <td>"+ rs.getString("dname") +"</td>");
				out.print("        <td>");
				out.print("            <a href='/servlet-04/dept/delete?dno="+deptno+"'>del</a>");
				//out.print("            <a href='javascript:void(0) on click='if(window.confirm(\"delete , ok ?\")){ window.location.href=\"/oa/dept/delete?dno="+deptno+"\";}'>del</a>");
				out.print("&nbsp;");
				out.print("            <a href='edit.html'>edit</a>");
				out.print("&nbsp;");
				out.print("            <a href='detail.html'>detail</a>");
				out.print("        </td>");
				out.print("    </tr>");		
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			//释放资源
			if (rs != null) {
				try {
					rs.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if (ps != null) {
				try {
					ps.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
				
			}
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
		out.print("</table>");
		out.print("</body>");
		out.print("</html>");
		
	}

	

}
