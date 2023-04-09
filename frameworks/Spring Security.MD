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
    3. Cache Control (can be overridden later by your application to allow caching of your static resources)
    4. X-XSS-Protection integration
    5. X-Frame-Options integration to help prevent Clickjacking
- Integrate with the following Servlet API methods:
    1. HttpServletRequest#getRemoteUser()
    2. HttpServletRequest.html#getUserPrincipal()
    3. HttpServletRequest.html#isUserInRole(java.lang.String)
    4. HttpServletRequest.html#login(java.lang.String, java.lang.String)
    5. HttpServletRequest.html#logout()
# Architecture
- FilterChain -> Filter -> DelegatingFilterProxy -> FilterChainProxy -> SecurityFilterChain -> Security Filters 
- SecurityFilterChain is multipel, which one should be used is depend on the url pattern.
## Saving Requests Between Authentication
- If you want to replay the original request. you can save request in the RequestCache. So you can continue your request after login
# Authentication
## Servlet Authentication Architecture
- SecurityContextHolder - The SecurityContextHolder is where Spring Security stores the details of who is authenticated.
- SecurityContext - is obtained from the SecurityContextHolder and contains the Authentication of the currently authenticated user.
- Authentication - Can be the input to AuthenticationManager to provide the credentials a user has provided to authenticate or the current user from the SecurityContext.
- GrantedAuthority - An authority that is granted to the principal on the Authentication (i.e. roles, scopes, etc.)
- AuthenticationManager - the API that defines how Spring Security’s Filters perform authentication.
- ProviderManager - the most common implementation of AuthenticationManager.
- AuthenticationProvider - used by ProviderManager to perform a specific type of authentication.
- Request Credentials with AuthenticationEntryPoint - used for requesting credentials from a client (i.e. redirecting to a log in page, sending a WWW-Authenticate response, etc.)
- AbstractAuthenticationProcessingFilter - a base Filter used for authentication. This also gives a good idea of the high level flow of authentication and how pieces work together.
## Username/Password
### Reading Username/Password
- Form: we can designate a login page.
- Basic
- Digest
### Password Storage
- Spring Security’s PasswordEncoder interface is used to perform a one-way transformation of a password to let the password be stored securely
- Password Storage Format
```
{id}encodedPassword
```
- Encode with Spring Boot CLI
```
spring encodepassword password
```
#### In-Memory
```
@Bean
public UserDetailsService users() {
	UserDetails user = User.builder()
		.username("user")
		.password("{bcrypt}$2a$10$GRLdNijSQMUvl/au9ofL.eDwmoohzzS7.rmNSJZ.0FxO/BTk76klW")
		.roles("USER")
		.build();
	UserDetails admin = User.builder()
		.username("admin")
		.password("{bcrypt}$2a$10$GRLdNijSQMUvl/au9ofL.eDwmoohzzS7.rmNSJZ.0FxO/BTk76klW")
		.roles("USER", "ADMIN")
		.build();
	return new InMemoryUserDetailsManager(user, admin);
}
```
#### JDBC
```
@Bean
UserDetailsManager users(DataSource dataSource) {
    UserDetails user = User.builder()
            .username("user")
            .password("{bcrypt}$2a$10$lnyur84zUZ6oOENgTVtqW.581UZ583PS73XCdZdwJp6c9k4ytNVji")
            .roles("USER")
            .build();
    UserDetails admin = User.builder()
            .username("admin")
            .password("{bcrypt}$2a$10$GRLdNijSQMUvl/au9ofL.eDwmoohzzS7.rmNSJZ.0FxO/BTk76klW")
            .roles("USER", "ADMIN")
            .build();
    JdbcUserDetailsManager users = new JdbcUserDetailsManager(dataSource);
    users.createUser(user);
    users.createUser(admin);
    return users;
}
```
#### Key Classes
- UserDetails: is returned by the ```UserDetailsService```. The ```DaoAuthenticationProvider``` validates the ```UserDetails``` and then returns an ```Authentication```. The principle of it is UserDetails.
- UserDetailsService: is used by ```DaoAuthenticationProvider``` for retrieving a username, a password, and other attributes for authenticating with a username and password. There are two implementations:
    1. JdbcUserDetailsManager
    2. InMemoryUserDetailsManager
    3. We can also custom our implementation.
- DaoAuthenticationProvider

![DaoAuthenticationProvider Usage](pictures/spring%20security/DaoAuthenticationProvider.png)
#### LDAP Authentication
- Lightweight Directory Access Protocol is often used by organizations as a central repository for user information and as an authentication service. It can also be used to store the role information for application users.
## Persistence
- Upon authenticating the user, the user is associated to a new session id to prevent session fixation attacks.
### persistence relevent Classes
- SecurityContextRepository, implementations:
    1. HttpSessionSecurityContextRepository
    2. RequestAttributeSecurityContextRepository
    3. NullSecurityContextRepository
    4. DelegatingSecurityContextRepository
- SecurityContextPersistenceFilter -> SecurityContextHolder -> SecurityContextRepository
- SecurityContextHolderFilter -> SecurityContextHolder -> SecurityContextRepository
## Session Management
## Remember Me