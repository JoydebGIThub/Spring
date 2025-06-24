# MVC kind of design pattern
- We use the `design pattern` to solve the common problems. Common problem like `keeping the data in organized way`.
- MVC used to organized the code for any web or desktop applications

### Model View Controller
- `Model`: It holds the data within the application. May have business logic: Java Bean
- `View`: It is responsible for representing presentation logic or UI interface.
- `Controller`: It intercept the request and sent to application and updates the changes to view. It glues to the view and vice versa.

**Spring MVC**: Gives readymade controller- `DispatcherServlet` or `FrontController`: mapped in web.xml file.

# Servlet:
Servlet is an interface. 
- For every single request we need to create different servlet.
- So, for resolving the issue they are going to use `MVC`. We can create a single class as controller instead of createing different class for different funtionality.
- We are organizing the code by using controller.

#### Servlet Flow:
Create web application:
- File -> new -> dynamic web project

![image](https://github.com/user-attachments/assets/ab219c2b-4979-4fa3-8660-967fcd4e503b)

- Every web project has the `servers`. Where we are going to **deploy the applications**. We use `tomcat server`
- In the project there are 2 important floder `java resource folder` and `WebContent`

![image](https://github.com/user-attachments/assets/d9d9bc25-bd9a-47af-9e21-e328c5bb93b7)

- In `java resource folder` we store all the java related code.
- In `WebContent` we store all the file related to `UI`.
  - It has configuration file called: `META-INF`
  - and another important folder is `WEB-INF` here in `lib` folder we have all the `libraries` all the `jar` file required for the web project.
  - here we add a file called `web.xml`
 
![image](https://github.com/user-attachments/assets/79a9ef19-82d7-4c6b-9921-98e6e319b4ce)

##### web.xml
It is the entry point where we store all the `mapping`. Here we metion the welcome page and also mentain the servlet mapping.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns="http://java.sun.com/xml/ns/javaee" 
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee,http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" version="3.0">
  <display-name>Spring3 MVC Application</display-name>
  <welcome-file-list>
      <welcome-file>login.html</welcome-file>
  </welcome-file-list>
  <servlet>
    <servlet-name>spring-web</servlet-name>
    <servlet-class>com.accenture.lkm.servlet.LoginServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>spring-web</servlet-name>
    <url-pattern>/Login</url-pattern>
  </servlet-mapping>
</web-app>
```

##### Login.html
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<form action="Login">
	<center>
	<h2>Login</h2>
		<table>
			<tr>
				<td>User Name</td>
				<td><input type="text" name="uname"> </td>
			</tr>
			<tr>
				<td>Password</td>
				<td><input type="password" name="upassword"> </td>
			</tr>
		</table>
		<input type="submit" value="Submit"/>
	</center>
</form>
</body>
</html>
```
- After submit the file is called the servlet mapping in the `web.xml`
- Through that servlet mapping it will called `LoginServlet.class`

![image](https://github.com/user-attachments/assets/05423210-0a10-4656-991a-79d6db74a6f6)

- DAO layer

![image](https://github.com/user-attachments/assets/fa46bc74-4c78-4fd9-acc0-7dc5d7cd8268)

- This class will configure everything and then show the `success.html` if the login is true if not then `failure.html`




