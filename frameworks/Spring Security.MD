# Overview
- Spring Security is a framework that provides authenticaiton, authorization and protection against common attacks.
- With first class support for securing both imperative and reactive applications.
# Getting Started (Servlet Application) (6.0.2)
- pom.xml
```
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-security</artifactId>
	</dependency>
</dependencies>
```
## Spring Boot Auto configuration
- Enables Spring Security's default configuration, which creates a servlet Filter as a bean named ```springSecurityFilterChain```. This bean is responsible for all the security:
    1. protecting the application URLs
    2. validating submitted username and passwords
    3. redirecting to the login form
- Create a ```UserDetailService``` bean with a username of user and a randomly generated password that is logged to the console.
- Registers the Filter with a bean named springSecurityFilterChain with the Servlet container for every request.
## A summary of the features
- Require an authenticated user for any interaction with the application.
- Generate a default login form for you.
- Let the user with a username and password that is logged to the console to authenticate with form-based authentication
- Protects the password storage with BCrypt
- Lets the user log out
- **CSRF(Cross-Site Request Forgery)** attack prevention
- **Session Fixation** protection
- Security Header integration
    1. HTTP Strict Transport Security for secure requests
    2. X-Content-Type-Option integration