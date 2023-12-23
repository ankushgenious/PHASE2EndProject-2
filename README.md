# PHASE2EndProject-2
# Railway Crossing Status 


import java.io.IOException; 
import   javax.servlet.ServletException; 
import  javax.servlet.annotation.WebServlet; 
import   javax.servlet.http.HttpServlet; 
import javax.servlet.http.HttpServletRequest; 
import javax.servlet.http.HttpServletResponse;

	import org.hibernate.Session;
	import org.hibernate.SessionFactory;
	@WebServlet("/AddToFavController")
	public class AddToFavController extends HttpServlet {
	private static final long serialVersionUID = 1L; 17

 protected void doGet(HttpServletRequest request, HttpServletResponse response) 20 throws ServletException, IOException {
	String crossingName = request.getParameter("crossingName");
	String crossingStatus = request.getParameter("crossingStatus");
	String personInCharge = request.getParameter("personInCharge");
	String trainSchedule = request.getParameter("trainSchedule");
	String landmark = request.getParameter("landmark");
	String address = request.getParameter("address"); 28
	AddFavCrossing afc = new AddFavCrossing();
	afc.setcName(crossingName);
	afc.setStatus(crossingStatus);
	afc.setpName(personInCharge);
	afc.setTrainSchedule(trainSchedule);
	afc.setLandmark(landmark);
	afc.setAddress(address); 

	SessionFactory sf = HibDbCon.getSessionFactory();
	Session session = sf.openSession(); 40
	session.beginTransaction();
	session.save(afc);
	session.getTransaction().commit();
	session.close(); 45
        response.sendRedirect("Home.jsp"); 47
 }
 protected void doPost(HttpServletRequest request, HttpServletResponse response) 
 throws ServletException, IOException {

 doGet(request, response); 
 }

 }
 
DeleteCrossing.java	



	import java.io.IOException;
	import javax.servlet.ServletException;
        import javax.servlet.annotation.WebServlet; 
	import javax.servlet.http.HttpServlet; 
        import javax.servlet.http.HttpServletRequest; 
        import javax.servlet.http.HttpServletResponse;

	import org.hibernate.Session;
	import org.hibernate.SessionFactory;
	import org.hibernate.query.Query; 14
	@WebServlet("/deleteCrossing")
	public class DeleteCrossing extends HttpServlet
 { 

         protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	 throws ServletException, IOException {

        String cName = request.getParameter("crossingName"); 
	SessionFactory sf = HibDbCon.getSessionFactory();
	Session session = sf.openSession();
	session.beginTransaction();
	Query q = session.createQuery("delete from CrossingDetails where cName =:name");
	q.setParameter("name", cName);
	q.executeUpdate();
	session.getTransaction().commit();
	response.sendRedirect("AdminHomePage.jsp"); 
 }
 protected void doPost(HttpServletRequest request, HttpServletResponse response) 
 throws ServletException, IOException {

 doGet(request, response); 
 }

 }
 
HibDbCon.java	

import   org.hibernate.SessionFactory;
import  org.hibernate.boot.Metadata; 
import org.hibernate.boot.MetadataSources;
import org.hibernate.boot.registry.StandardServiceRegistry;
import org.hibernate.boot.registry.StandardServiceRegistryBuilder;
public class HibDbCon { 
private static SessionFactory sessionfactory; 
 { 
 StandardServiceRegistry sr = new StandardServiceRegistryBuilder().configure 14 ("hibernate.cfg.xml").build();
Metadata mt = new MetadataSources(sr).getMetadataBuilder().build(); 
sessionfactory = mt.getSessionFactoryBuilder().build(); 
}

public static SessionFactory getSessionFactory() {
return sessionfactory; 
}

 }


 
HomePage.java	

1
2 import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import  javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/HomePage")
public class HomePage extends HttpServlet { 13

protected void doGet(HttpServletRequest request, HttpServletResponse response) 
throws ServletException, IOException {
PrintWriter out = response.getWriter(); 
out.println("Welcome to home page");
response.sendRedirect("Home.jsp"); 
}
protected void doPost(HttpServletRequest request, HttpServletResponse response) 
throws ServletException, IOException {

doGet(request, response); 
}

 }

 



 import java.io.IOException;
 import java.util.List;

import  javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpFilter;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

import java.io.IOException;

@WebFilter("/HomePage")
public class LoginToHomeFilter extends HttpFilter implements Filter  
{ 
public LoginToHomeFilter() {
super();
}
 public void destroy() { 
 }
 public void doFilter(ServletRequest request, ServletResponse response,  FilterChain chain) throws IOException, ServletException {
	HttpServletRequest req = (HttpServletRequest)request;
	HttpServletResponse res = (HttpServletResponse)response; 
	String name = req.getParameter("email");
	String pwd = req.getParameter("password"); 
	SessionFactory sf = HibDbCon.getSessionFactory();
	Session session = sf.openSession(); 
	Query<Object[]> q = session.createQuery("select email, password from User");
	List<Object[]> resultList = q.getResultList(); 
	boolean found = false;
	for (Object[] userCredentials : resultList) {
	String email = (String) userCredentials[0];
	String password = (String) userCredentials[1];
	if (email.equals(name) && password.equals(pwd)) {
	found = true;
        chain.doFilter(request, response);
	break; // Exit the loop since we found a match
 

 }
 }

if (!found) { 

 if(name.equalsIgnoreCase("admin123@gmail.com") && pwd.contentEquals  ("admin123")) {
res.sendRedirect("AdminHomePage.jsp");
	}
 else {
res.sendRedirect("index.jsp"); 71 }

 }

 }

public void init(FilterConfig fConfig) throws ServletException { 78
}

 }
 
RegisterNullFilter.java	


	import java.io.IOException;
	import java.io.PrintWriter;
	import javax.servlet.Filter;
	import javax.servlet.FilterChain;
	import javax.servlet.FilterConfig;
	import javax.servlet.ServletException;
	import javax.servlet.ServletRequest;
	import javax.servlet.ServletResponse;
	import javax.servlet.annotation.WebFilter;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse; 13
	@WebFilter("/URegister")
	public class RegisterNullFilter implements Filter { 16
 public void destroy() { 
 }

public void doFilter(ServletRequest request, ServletResponse response,  FilterChain chain)
throws IOException, ServletException { 23
HttpServletRequest Request = (HttpServletRequest) request; 25 HttpServletResponse Response = (HttpServletResponse) response; 26
PrintWriter out = Response.getWriter(); 28
String username = Request.getParameter("username");
String password = Request.getParameter("password");
String email = Request.getParameter("email"); 32

if (username == null || username.isEmpty() || password == null || 35 password.isEmpty() || email == null
 || email.isEmpty()) { 

out.println("<html><body>");
out.println(username+", "+password+", "+email);
out.println("<h4>Fields cannot be empty..</h4>");
out.println("<a href='Register.jsp'>Click here to Register</a>");
out.println("</body></html>");
	} 
 else {
chain.doFilter(request, response); 46 }
 }

public void init(FilterConfig fConfig) throws ServletException { 
}
 }

 
RemoveFromFavController.java	


import java.io.IOException;
import        javax.servlet.ServletException;
 import  javax.servlet.annotation.WebServlet;
 import   javax.servlet.http.HttpServlet;
 import javax.servlet.http.HttpServletRequest;
 import javax.servlet.http.HttpServletResponse;
import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

@WebServlet("/RemoveFromFavController")
public class RemoveFromFavController extends HttpServlet {
private static final long serialVersionUID = 1L; 

protected void doGet(HttpServletRequest request, HttpServletResponse response)  throws ServletException, IOException {

String crossingName = request.getParameter("crossingName"); 23
SessionFactory sf = HibDbCon.getSessionFactory();
Session session = sf.openSession();
session.beginTransaction();
Query<?> q = session.createQuery("DELETE FROM AddFavCrossing WHERE cName =
:crossingName");
q.setParameter("crossingName", crossingName);
q.executeUpdate();
session.getTransaction().commit();
session.close();
response.sendRedirect("FavCrossings.jsp"); 33

 }
 protected void doPost(HttpServletRequest request, HttpServletResponse response)  throws ServletException, IOException {
	// TODO Auto-generated method stub
	doGet(request, response); 40 }

 }
 
SearchCrossingRetriever.java	


 import java.io.IOException; 
 import java.io.PrintWriter;
 import java.util.List;

import  javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

import com.daoClasses.CrossingDetails;

@WebServlet("/SearchCrossingRetriever")
public class SearchCrossingRetriever extends HttpServlet {
private static final long serialVersionUID = 1L; 23
protected void doGet(HttpServletRequest request, HttpServletResponse response)  throws ServletException, IOException {
PrintWriter out = response.getWriter();
String sName = request.getParameter("searchCrossing"); 28
SessionFactory sf = HibDbCon.getSessionFactory();
Session session = sf.openSession();
Query<CrossingDetails> query = session.createQuery("from CrossingDetails where cName like :searchName");
query.setParameter("searchName", "%" + sName + "%");
List<CrossingDetails> crossings = query.getResultList(); 34
	HttpSession httpsession = request.getSession();
	httpsession.setAttribute("Crossings", crossings);
	response.sendRedirect("SearchCrossing.jsp?prop=block"); 38

 }
 protected void doPost(HttpServletRequest request, HttpServletResponse response) 42 throws ServletException, IOException {

 doGet(request, response); 45 }

 }

 
UserRegister.java	


	import java.io.IOException;
	import javax.servlet.ServletException;
	import javax.servlet.annotation.WebServlet;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	import javax.servlet.http.HttpSession; 
         import com.daoClasses.User; 
	import org.hibernate.Session;
	import org.hibernate.SessionFactory; 
	@WebServlet("/URegister")
	public class UserRegister extends HttpServlet {
	private static final long serialVersionUID = 1L; 
        protected void doGet(HttpServletRequest request, HttpServletResponse response) 20 throws ServletException, IOException {

	String username = request.getParameter("username");
	String password = request.getParameter("password");
	String email = request.getParameter("email"); 
        User userDetails = new User(); 
	userDetails.setUname(username);
	userDetails.setPassword(password);
	userDetails.setEmail(email);
	//System.out.println(username+"; "+password+": "+email); 32
	SessionFactory sf = HibDbCon.getSessionFactory();
	Session session = sf.openSession(); 35
	session.beginTransaction();
	session.save(userDetails);
	session.getTransaction().commit();
	session.close();
	// HttpSession httpSession = request.getSession();
	// httpSession.setAttribute("userdetails", userDetails);
	response.sendRedirect("LoginPage.jsp"); 43
 }
 protected void doPost(HttpServletRequest request, HttpServletResponse response) 46 throws ServletException, IOException {

 doGet(request, response); 49 }

 }

 

package com.daoClasses;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table; 
@Entity
@Table(name="favcrossings")
public class AddFavCrossing { 10
@Id
@Column(name="crossing_name")
private String cName;
@Column(name="crossing_status")
private String status;
@Column(name="person_name")
private String pName;
@Column(name="train_schedule")
private String trainSchedule;
@Column(name="landmark")
private String landmark;
@Column(name="address")
private String address; 24
public String getcName() {
return cName; 27 }

public void setcName(String cName) {
this.cName = cName; 31 }

public String getpName() {
return pName; 35 }

public void setpName(String pName) {
this.pName = pName; 39 }

public String getTrainSchedule() {
return trainSchedule; 43 }

public void setTrainSchedule(String trainSchedule2) {
this.trainSchedule = trainSchedule2; 47 }

public String getLandmark() {
return landmark; 51 }

public void setLandmark(String landmark) {
54	this.landmark = landmark; 55 }
56
57	public String getAddress() {
58	return address; 59 }
 

	public void setAddress(String address) {
	this.address = address; 63 }

	public String getStatus() {
	return status; 67
}
	public void setStatus(String status) {
	this.status = status; 72 }
 }

 

 package com.daoClasses; 2

	import javax.persistence.Column;
	import javax.persistence.Entity;
6	import javax.persistence.Id; 7
8	@Entity
9	public class CrossingDetails { 10
11	@Id
12	@Column(name="crossing_name")
13	private String cName;
14	@Column(name="crossing_status")
15	private String status;
16	@Column(name="person_name")
17	private String pName;
18	@Column(name="train_schedule")
19	private String trainSchedule;
20	@Column(name="landmark")
21	private String landmark;
22	@Column(name="address")
23	private String address; 24
25	public String getcName() {
26	return cName; 27 }
28
29	public void setcName(String cName) {
30	this.cName = cName; 31 }
32
33	public String getpName() {
34	return pName; 35 }
36
37	public void setpName(String pName) {
38	this.pName = pName; 39 }
40
41	public String getTrainSchedule() {
42	return trainSchedule; 43 }
44
45	public void setTrainSchedule(String trainSchedule2) {
46	this.trainSchedule = trainSchedule2; 47 }
48
49	public String getLandmark() {
50	return landmark; 51 }
52
53	public void setLandmark(String landmark) {
54	this.landmark = landmark; 55 }
56
57	public String getAddress() {
58	return address; 59 }
 

60
61	public void setAddress(String address) {
62	this.address = address; 63 }
64
65 public String getStatus() { 66
67 return status; 68 }
69
70	public void setStatus(String status) {
71	this.status = status; 72 }
73 }
74
 
User.java	

1	package com.daoClasses;
2	import javax.persistence.Column;
3	import javax.persistence.Entity;
4	import javax.persistence.Id; 5
6	@Entity
7	public class User { 8
9	@Id
10	@Column(name="email")
11	private String email;
12	@Column(name="username")
13	private String uname;
14	@Column(name="password")
15	private String password;
16	public String getEmail() {
17	return email; 18 }
19	public void setEmail(String email) {
20	this.email = email; 21 }
22	public String getUname() {
23	return uname; 24 }
25	public void setUname(String uname) {
26	this.uname = uname; 27 }
28	public String getPassword() {
29	return password; 30 }
31	public void setPassword(String password) {
32	this.password = password; 33 }
34
35
36
37 }
38
 
hibernate.cfg.xml	

1	<?xml version="1.0" encoding="UTF-8"?>
2	<!DOCTYPE hibernate-configuration PUBLIC
3	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
4	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
5	<hibernate-configuration>
6	<session-factory>
7	<property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
9	<property name="hibernate.connection.password">Nodan7253@?</property>
10	<property
11	name="hibernate.connection.url">jdbc:mysql://localhost:3306/railwaycrossingdb</property>
13	<property name="hibernate.connection.username">root</property>
14	<property
15	name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>
16
17 <property name="hbm2ddl.auto">update</property> 
19	<mapping class="com.daoClasses.User"/>
20	<mapping class="com.daoClasses.CrossingDetails"/>
21	<mapping class="com.daoClasses.AddFavCrossing"/>
22	</session-factory>
23	</hibernate-configuration>
 

1	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
2	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
4 <html> 
<head>
6 <meta charset="ISO-8859-1">  <title>Insert title here</title>  <style>
9	#header{
10	display:flex;
11	justify-content:space-between;
12	align-items:center; 13 }
14	span{
15	font-size:10px; 16 }
17	button{
18	height:14px;
19	display:flex;
20	justify-content:center;
21	align-items:center;
22	text-align:center;
23	border:none; 24
25 }
26 .AdminHomeControls{ 27
28 display:flex; 29 }
30	.AdminHomeControls button{
31	margin-right:15px; 32 }
33	#tableContainer{
34	margin:10px; 35 }
36	th, td {
37	padding: 8px;
38	text-align: left;
39	border-bottom: 1px solid gray; 40 }
41
42 </style> 43
44	</head>
45	<body>
46	<div id="header">
47	<h3>Admi Home Page</h3>
48	<button id="logoutButton"><span>Logout</span></button>
49	</div> 50
51	<div class="AdminHomeControls">
52	<button id="adminHome"><span>Home</span></button>
53	<button id="addCrossing"><span>Add Railway Crossings</span></button>
54	</div> 55
56	<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
57	<%@ page import="java.sql.*" %> 58
59	<sql:setDataSource var="dataSource" driver="com.mysql.cj.jdbc.Driver"
 

60	url="jdbc:mysql://localhost:3306/railwaycrossingdb" user="root"
61	password="Ammanana@789" />
62
63 <sql:query dataSource="${dataSource}" var="result"> 64
65
66	select * from crossingdetails
67	</sql:query> 68
69
70	<%int count=0; %>
71	<div id="tableContainer">
72	<table>
73	<tr>
74	<th>Sr.No</th>
75	<th>Name</th>
76	<th>Address</th>
77	<th>Landmark</th>
78	<th>Train Schedule</th>
79	<th>Person In Charge</th>
80	<th>Action</th> 81 </tr>
82	<c:forEach var="row" items="${result.rows}">
83	<tr>
84 <td><%=++count%></td>
85	<td>${row.crossing_name}</td>
86	<td>${row.address}</td>
87	<td>${row.landmark}</td>
88	<td>${row.train_schedule}</td>
89	<td>${row.person_name}</td>
90	<td>
91	<div style="display: flex; flex-direction:row">
92	<form action="" method="post">
93	<input type="hidden" name="crossingName" 94 value="${row.crossing_name}">
95	<input type="submit" value="Update" id="deleteCrossing">
96	</form>
97	<form action="deleteCrossing" method="post">
98	<input type="hidden" name="crossingName" 99 value="${row.crossing_name}">
100	<input type="submit" value="Delete" id="deleteCrossing">
101	</form>
102	</div>
103 </td>
104 </tr>
105	</c:forEach>
106	</table>
107	</div>
108	<script> 109
110	const logoutButton = document.getElementById('logoutButton');
111	logoutButton.addEventListener('click', function() {
112	window.location.href = 'LogoutPage.jsp'; 113 });
114
115	const homeButton = document.getElementById("adminHome");
116	homeButton.addEventListener('click', function(e) { 117 window.location.href="AdminHomePage.jsp";
118
 

119 });
120
121	const acButton = document.getElementById("addCrossing");
122	acButton.addEventListener('click', function(e) {
123	window.location.href="AddRailwayCrossing.jsp"; 124 });
125
126 </script> 127
128	</body>
129	</html> 130
 

1	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
2	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
4 <html> 5 <head>
6 <meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 <style>
9	#header{
10	display:flex;
11	justify-content:space-between;
12	align-items:center; 13 }
14	span{
15	font-size:10px; 16 }
17	button{
18	height:14px;
19	display:flex;
20	justify-content:center;
21	align-items:center;
22	text-align:center;
23	border:none; 24
25 }
26 .Controllers{ 27
28 display:flex; 29 }
30	.Controllers button{
31	margin-right:15px; 32 }
33
34	#crossingDetails{
35	margin-top:20px; 36 }
37	#crossingDetails p{
38	margin:0px;
39	padding:8px;
40	color:gray;
41	font-size:small; 42 }
43	#crossingDetails h5{
44	margin:0px;
45	padding:8px;
46	font-size:medium; 47 }
48	#RemoveFav{
49	width:120px;
50	margin:8px;
51	margin-top:0px;
52	background-color:white;
53	color:gray;
54	font-size:8px;
55	font-weight:bold;
56	height:20px; 57 }
58	#crossingDetails span{
59	color:black;
 

60	font-size:15px;
61	font-weight:bold; 62
63 }
64 </style> 65
66 </head> 67 <body> 68
69	<div id="header">
70	<h3>Favourite Railway Crossing</h3>
71	<button id="logoutButton"><span>Logout</span></button>
72	</div> 73
74	<div class="Controllers">
75	<button id="Home"><span>Home</span></button>
76	<button id="SearchCrossing"><span>Search Crossings</span></button>
77	</div> 78
79	<script type="text/javascript">
80	const searchButton = document.getElementById("SearchCrossing");
81	searchButton.addEventListener('click', function(){
82	window.location.href = "SearchCrossing.jsp"; 83 })
84 </script> 85
86	<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
87	<%@ page import="java.sql.*" %> 88
89 <sql:setDataSource var="dataSource" driver="com.mysql.cj.jdbc.Driver" 90 url="jdbc:mysql://localhost:3306/railwaycrossingdb" user="root"
91 password="Ammanana@789" />
92
93	<sql:query dataSource="${dataSource}" var="result">
94	select * from favcrossings; 95 </sql:query>
96
97 <c:forEach items="${result.rows}" var="row"> 98
99	<div id="crossingDetails" style="border: 1px solid gray;">
100	<h5>"${row.crossing_name}"</h5>
101	<p>Crossing status: <span style="color: white; background-color: 102 ${row.crossing_status == 'open' ? 'green' : 'red'}; padding:2px; 103 height:7px;">"${row.crossing_status}"</span></p>
104	<p>Person In Charge: <span>"${row.person_name}"</span></p>
105	<p>Train Schedule: <span>"${row.train_schedule}"</span></p>
106	<p>Landmark: <span>"${row.landmark}"</span></p>
107	<p>Address: <span>"${row.address}"</span></p> 108
109	<form action="RemoveFromFavController" method="post">
110	<input type="hidden" name="crossingName" 111 value="${row.crossing_name}">
112	<input type="submit" value="REMOVE FROM FAVOURITE" id="RemoveFav">
113	</form>
114	</div> 115
116 </c:forEach> 117
118	<script type="text/javascript">
 

	const homeButton = document.getElementById("Home");
	homeButton.addEventListener('click', function() {
	window.location.href="Home.jsp"; 122
 });

	const logoutButton = document.getElementById("logoutButton");
	logoutButton.addEventListener('click', function(){ 127
 window.location.href="LogoutPage.jsp"; 129 });
</script> 131
	</body>
	</html>
 

	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
 <html> 
	 <head>
<meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 <style>
	#header{
	display:flex;
11	justify-content:space-between;
12	align-items:center; 13 }
14	span{
15	font-size:10px; 16 }
17	button{
18	height:14px;
19	display:flex;
20	justify-content:center;
21	align-items:center;
22	text-align:center;
23	border:none; 24
25 }
26 .crossing{ 
28 display:flex; 
}
	.crossing button{
	margin-right:15px; 
}
33	#crossingDetails{
34	margin-top:20px; 
}
36	#crossingDetails p{
37	margin:0px;
38	padding:8px;
39	color:gray;
40	font-size:small;
41 }
42	#crossingDetails h5{
43	margin:0px;
44	padding:8px;
45	font-size:medium; 46 }
47	#AddFav{
48	width:120px;
49	margin:8px;
50	margin-top:0px;
51	background-color:white;
52	color:gray;
53	font-size:8px;
54	font-weight:bold;
55	height:20px; 56 }
57	#crossingDetails span{
58	color:black;
59	font-size:15px;
 

60	font-weight:bold; 61 }
62 </style> 63
64	</head>
65	<body>
66	<div id="header">
67	<h3>User HomePage</h3>
68	<button id="logoutButton"><span>Logout</span></button>
69	</div> 70
71	<div class="crossing">
72	<button id="allCrosses"><span>All</span></button>
73	<button id="favCrosses"><span>Favourite Crossings</span></button>
74	</div> 75
76	<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
77	<%@ page import="java.sql.*" %> 78
79 <sql:setDataSource var="dataSource" driver="com.mysql.cj.jdbc.Driver" 80 url="jdbc:mysql://localhost:3306/railwaycrossingdb" user="root"
81 password="Ammanana@789" />
82
83	<sql:query dataSource="${dataSource}" var="result">
84	select * from crossingdetails
85	</sql:query> 86
87
88
89	<c:forEach var="row" items="${result.rows}">
90	<div id="crossingDetails" style="border: 1px solid gray;">
91	<h5>${row.crossing_name}"</h5>
92	<p>Crossing status: <span style="color: white; background-color: 93 ${row.crossing_status == 'open' ? 'green' : 'red'}; padding:2px; 94 height:7px;">"${row.crossing_status}"</span></p>
95	<p>Person In Charge: <span>"${row.person_name}"</span></p>
96	<p>Train Schedule: <span>"${row.train_schedule}"</span></p>
97	<p>Landmark: <span>"${row.landmark}"</span></p>
98	<p>Address: <span>"${row.address}"</span></p> 99
100	<form action="AddToFavController" method="post">
101	<input type="hidden" name="crossingName" value="${row.crossing_name}">
102	<input type="hidden" name="crossingStatus" 103 value="${row.crossing_status}">
104	<input type="hidden" name="personInCharge" value="${row.person_name}">
105	<input type="hidden" name="trainSchedule" 106 value="${row.train_schedule}">
107	<input type="hidden" name="landmark" value="${row.landmark}">
108	<input type="hidden" name="address" value="${row.address}">
109	<input type="submit" value="ADD TO FAVOURITE" id="AddFav">
110	</form>
111	</div> 112
113
114 </c:forEach> 115
116	<script>
117	// Get a reference to the submit button
118	const submitButton = document.getElementById('logoutButton');
 

119	
120	// Add a click event listener to the submit button
121	submitButton.addEventListener('click', function() {
122	// Redirect to the specific page
123	window.location.href = 'LogoutPage.jsp'; 124 });
125
126
127	const homeButton = document.getElementById("favCrosses");
128	homeButton.addEventListener('click', function(e) {
129	window.location.href="FavCrossings.jsp"; 130
131 });
132 </script> 133
134	</body>
135	</html>
 

1	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
2	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
4 <html> 5 <head>
6 <meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 </head>
9 <body> 10 <style>
11	body {
12	display: flex;
13	justify-content: center;
14	align-items: center;
15	height: 100vh; 16 }
17
18	div {
19	text-align: center; 20 }
21
22	table th {
23	text-align: left; 24 }
25
26	table td {
27	text-align: left; 28 }
29
30	table tr {
31	margin-bottom: 10px; 32 }
33	table td input {
34	margin-left: 40px;
35	width:150px 36 }
37	#register {
38	background-color: lime;
39	width:150px;
40	margin-top:10px; 41 }
42	table td span{
43	font-size:10px;
44	align-items:center;
45	justify-content:center;
46	display:flex;
47	margin-left:25px; 48 }
49	</style>
50	</head> 51 </body> 52 <div>
53	<form action="URegister" method="post">
54	<table>
55	<tr>
56	<th>Enter name:</th> 57 </tr>
58	<tr>
59	<td><input type="text" id="username" name="username"/></td>
 

60 </tr>
61	<tr>
62	<th>Enter Email:</th> 63 </tr>
64	<tr>
65	<td><input type="email" id="email" name="email"/></td> 66 </tr>
67
68	<tr>
69	<th>Enter Password:</th> 70 </tr>
71	<tr>
72	<td><input type="password" id="password" name="password"/></td> 73 </tr>
74	<tr>
75	<td><input type="submit" value="Register" id="register"></td> 76 </tr>
77 <tr><td><span>Already have account?<a href="LoginPage.jsp">Sign in 78 here</a></span></td></tr>
79	</table>
80	</form>
81	</div>
82	</body> 83
84 </html>
 

1	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
2	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
4 <html> 5 <head>
6 <meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 <style>
9
10	body{
11	height:100vh;
12	text-align:center;
13	align-items:center;
14	justify-content:center; 15 display:flex;
16 }
17	table th{
18	text-align:left; 19 }
20	table td{
21	text-align:left; 22 }
23	table tr {
24	margin-bottom: 10px; 25 }
26	table td input {
27	margin-left: 20px;
28	width:150px 29 }
30	#login{
31	background-color: lime;
32	width:150px;
33	margin-top:10px; 34 }
35	table td span{
36	font-size:10px;
37	align-items:center;
38	justify-content:center;
39	display:flex;
40	margin-left:18px; 41 }
42 </style> 43
44	</head>
45	<body> 46
47	<form action="HomePage">
48	<table>
49	<tr><th>Enter Email: </th></tr>
50	<tr><td><input type="email" id="email" name="email"></td></tr>
51	<tr><th>Enter Password: </th></tr>
52	<tr><td><input type="password" id="password" name="password"></td></tr>
53	<tr><td><input type="submit" value="Login" name="login" 54 id="login"></td></tr>
55 <tr><td><span>Dont have account?<a href="index.jsp">Create new 56 account</a></span></td></tr>
57	</table>
58	</form> 59
 

60	</body>
61	</html> 62
 
LogoutPage.jsp	
	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
 <html> 
	 <head>
 <meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 <style>
	#logoutSec{
	display:flex;
	flex-direction:column;
12	align-items:center;
13	justify-content:center;
14	height: 100vh; 15 }
16	#logoutSec .linkText{
17	font-size:small;
18	font-weight:bold; 19 }
20 </style> 21
22	</head>
23	<body>
24	<div id="logoutSec">
25	<h3>Logged Out Successfully.. </h3>
26	<a href="LoginPage.jsp"><span style="font-style: oblique;" 27 class="linkText">Click here to Login again</span></a>
28	</div>
29	</body>
30	</html>
 

1	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
2	pageEncoding="ISO-8859-1"%> 3 <!DOCTYPE html>
4 <html> 5 <head>
6 <meta charset="ISO-8859-1"> 7 <title>Insert title here</title> 8 <style>
9 11
#header{
 display:flex;
justify-content:space-between;
 align-items:center;
 }
 span{
font-size:10px;
 }
 button{
 height:14px;
display:flex;
justify-content:center;
 align-items:center;
 text-align:center;
 border:none;
 }
 .Controllers{

 display:flex;
}
Controllers button{
 margin-right:15px;
 }
#searchContainer p{
 margin:0px;
font-size:12px;
 padding-top:10px;
padding-bottom:8px;
}
#searchContainer input{
width:200px;
border:1px solid cyan; 
margin-bottom:8px;
 }
#searchContainer button{
width:200px;
 height:20px;
 }
 #crossingDetails{
 margin-top:20px;
 }
#crossingDetails p{
 margin:0px;
padding:8px;
color:gray;
 font-size:small;
}
 #crossingDetails h5{
 

 margin:0px;
padding:8px;
font-size:medium;
 }
 #AddFav{
width:120px;
 margin:8px;
 margin-top:0px;
 background-color:white;
color:gray;
font-size:8px;
font-weight:bold;
 height:20px;
}
 #crossingDetails span{
 color:black;
 font-size:15px;
 font-weight:bold;
}

 </style> 83
	</head>
	<body> 
 <div id="header">
 <h3>Search Railway Crossing</h3>
 <button id="logoutButton"><span>Logout</span></button>
</div>
 <div class="Controllers">
 <button id="Home"><span>All</span></button>
 <button id="favCrossing"><span>Favourite Crossings</span></button> 95 96 </div>
 <div id="searchContainer">
 <form action="SearchCrossingRetriever"> 99 100 <p>Search Railway Crossing</p>
<input type="search" name="searchCrossing" id="searchCrossing"> 101 102 <button id="searchButton">Search</button>
</form>
</div>
 <%String prop = request.getParameter("prop");
	if(prop==null){
	prop="none";
 } 
 %> 
 <div id="resultContainer" style="display:<%=prop%>"> 
	 <c:forEach var="crossing" items="${Crossings}">
 <div id="crossingDetails" style="border: 1px solid gray;"> 
	 <h5>${crossing.cName}</h5>
 <p>Crossing status: <span style="color: white; background-color: 
			     ${crossing.status == 'open' ? 'green' : 'red'}; padding:2px; 
			     height:7px;">"${crossing.status}"</span></p>
 <p>Person In Charge: <span>${crossing.pName}</span></p>
 

 <p>Train Schedule: <span>${crossing.trainSchedule}</span></p> 
 <p>Landmark: <span>${crossing.landmark}</span></p>
 <p>Address: <span>${crossing.address}</span></p>

 <form action="AddToFavController" method="post">
 <input type="hidden" name="crossingName" value="${crossing.cName}"> 
	 <input type="hidden" name="crossingStatus" value="${crossing.status}"> 
	 <input type="hidden" name="personInCharge" value="${crossing.pName}">
	 <input type="hidden" name="trainSchedule"
	value="${crossing.trainSchedule}">
  <input type="hidden" name="landmark" value="${crossing.landmark}"> 
	 <input type="hidden" name="address" value="${crossing.address}"> 
	 <input type="submit" value="ADD TO FAVOURITE" id="AddFav">
 </form>
 </div>
 </c:forEach>
 </div>

 <script>
 const logoutButton = document.getElementById('logoutButton'); 
	 logoutButton.addEventListener('click', function() {
 window.location.href = 'LogoutPage.jsp';
	 });

 const homeButton = document.getElementById("Home"); 145 142 homeButton.addEventListener('click', function() {  window.location.href = "Home.jsp";
 });

 const favButton = document.getElementById("favCrossing"); 150 147 favButton.addEventListener('click', function() {
 window.location.href = "FavCrossings.jsp";
 });

 </script>
 </body>
</html>
