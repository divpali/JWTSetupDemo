# Getting Started

### Steps followed

1. setup Mysql DB connection

https://medium.com/@rodolfovmartins/how-to-install-mysql-on-mac-959df86a5319

change username and password for the DB you are creating

https://www.twilio.com/en-us/blog/beginner-mysql-database-java-spring-boot

mysql> CREATE DATABASE quotes_database;

mysql> USE quotes_database;


//change username and password and grant all permission

CREATE USER username@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON quotes_database.* TO username@'localhost';

download mysql workbench which is your MySQL client

2. Install dependencies

	implementation 'com.auth0:java-jwt:4.4.0'
	implementation 'io.jsonwebtoken:jjwt:0.12.5'
	implementation 'io.jsonwebtoken:jjwt-api:0.12.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.12.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.12.5'
	implementation 'org.springframework.security:spring-security-web:6.2.4'
	implementation 'org.springframework.security:spring-security-core:6.2.4'
	implementation 'org.springframework.security:spring-security-config:6.2.4'
	implementation 'org.springframework.boot:spring-boot-starter-security:3.2.5'

3. Create a User entity

4. Create a Data access layer for User entity - UserRepository
The function findByEmail() will be used later when implementing the user authentication.

5. Extend User Entity with authentication details
User entity must override UserDetails for manager user details related to authentication.
Spring Security provides an Interface named UserDetails

6. Create JWT Service
To generate, validate, and decode JSON web tokens, add methods from elated libraries
The methods that will be used are generateToken(), isTokenValid() and getExpirationTime().
To generate the JWT token, we need a secret key and the token expiration time whose values are read from application properties

### Create secret key:

node -e "console.log(require('crypto').randomBytes(32).toString('hex'))”

7. Override security configuration

We have to override HTTP basic authentication, to perform authentication by finding the user in our DB
and generate a JWT token when authentication succeeds

The userDetailsService() defines how to retrieve the user using the UserRepository that is injected.

The passwordEncoder() creates an instance of the BCryptPasswordEncoder() used to encode the plain user password.

The authenticationProvider() sets the new strategy to perform the authentication.

8. Create Authentication middleware - JwtAuthenticationFilter
If the token is invalid, reject the request
If the token is valid, extract the username, find the related user in the DB, and set it in the authentication context so you can access it in the application layer.


9. Configure the application requester filter to define what criteria is to be matched before forwarding a request to application middleware.
- no need to provide the CSRF token
- request URL path matching /auth/signup and /auth/login doesn't require authentication.
- Any other request URL path must be authenticated.
- stateless request - every request is treated as a new one even if it's from the same client or received earlier.
- The CORS configuration must allow only POST and GET requests. The CORS configuration must allow only POST and GET requests.

10. Create an authentication service

11. Create user registration and authentication routes
- create the routes /auth/signup and /auth/login for user registration and authentication rate the routes /auth/signup and /auth/login for user registration and authentication
- /auth/signup -> to register new User
- /auth/login -> authenticate with the user we registered

12. Create restricted endpoints to retrieve authenticated users. Put the JWT token in the authorization header of the request.

13. Customize authentication error messages
@RestControllerAdvice - handling exceptions in restful web service
define a centralized exception-handling mechanism for all @RestController classes
ensures that the error responses have a consistent structure

### Reference Documentation
https://medium.com/@tericcabrel/implement-jwt-authentication-in-a-spring-boot-3-application-5839e4fd8fac
