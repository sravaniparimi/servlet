package com.jnit;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class DoctorRegister extends HttpServlet {
	Connection con=null;
	PreparedStatement ps=null;
    public void init(ServletConfig config) {
    	try {
			Class.forName("com.mysql.jdbc.Driver");
			String url="jdbc:mysql://localhost:3306/jnit";
			String user="root";
			String pass="1234567890";
			con=DriverManager.getConnection(url,user,pass);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
        }
    public void doGet(HttpServletRequest request,HttpServletResponse response) throws IOException {
    	String name=request.getParameter("dname");
    	String password=request.getParameter("password");
    	String email=request.getParameter("email");
    	long phone=Long.parseLong(request.getParameter("phone"));
    	String specialisation=request.getParameter("specialisation");
    	
    	try {
            ps=con.prepareStatement("insert into doctor(name,password,email,phone,specialisation) values(?,?,?,?,?)");
			ps.setString(1, name);
			ps.setString(2, password);
			ps.setString(3, email);
			ps.setLong(4, phone);
			ps.setString(5, specialisation);
	   		PrintWriter pw=response.getWriter();
			int x=ps.executeUpdate();
			pw.println("<html><body bgcolor='wheat'><h1><center>");
			if(x!=0)
			{
				
				pw.println("Register Successfully");
				
			}
			pw.println("</center></h1></body></html>");
			
	} catch (SQLException e) {
			
			e.printStackTrace();
		}
    	
    	
    }
}
