SPRING BOOT TOKEN BASED AUTHENTICATON WITH SPRING SECURITY & JWT

Flow for User Signup & User Login with JWT auth


Spring SECURITY
– **Websecurityconfig** is the crux of our security implementation. It configures cors, csrf, session management, rules for protected resources. We can also extend and customize the default configuration that contains the elements below.
(WebSecurityConfigurerAdapter is deprecated from Spring 2.7.0, you can check the source code for update. More details at:
WebSecurityConfigurerAdapter Deprecated in Spring Boot)

– **UserDetailsService** interface has a method to load User by username and returns a UserDetails object that Spring Security can use for authentication and validation.

– **UserDetails** contains necessary information (such as: username, password, authorities) to build an Authentication object.

– **UsernamePasswordAuthenticationToken** gets {username, password} from login Request, AuthenticationManager will use it to authenticate a login account.

– **AuthenticationManager** has a DaoAuthenticationProvider (with help of UserDetailsService & PasswordEncoder) to validate UsernamePasswordAuthenticationToken object. If successful, AuthenticationManager returns a fully populated Authentication object (including granted authorities).

– **OncePerRequestFilter** makes a single execution for each request to our API. It provides a doFilterInternal() method that we will implement parsing & validating JWT, loading User details (using UserDetailsService), checking Authorizaion (using UsernamePasswordAuthenticationToken).

– **AuthenticationEntryPoint** will catch authentication error.
Repository contains UserRepository & RoleRepository to work with Database, will be imported into Controller.

Controller receives and handles request after it was filtered by OncePerRequestFilter.

– AuthController handles signup/login requests

– TestController has accessing protected resource methods with role based validations.

Understand the architecture deeply and grasp the overview more easier:

security: we configure Spring Security & implement Security Objects here.

WebSecurityConfig
(WebSecurityConfigurerAdapter is deprecated from Spring 2.7.0, you can check the source code for update. More details at:
WebSecurityConfigurerAdapter Deprecated in Spring Boot)

UserDetailsServiceImpl implements UserDetailsService
UserDetailsImpl implements UserDetails
AuthEntryPointJwt implements AuthenticationEntryPoint
AuthTokenFilter extends OncePerRequestFilter
JwtUtils provides methods for generating, parsing, validating JWT
controllers handle signup/login requests & authorized requests.

AuthController: @PostMapping(‘/signin’), @PostMapping(‘/signup’)
TestController: @GetMapping(‘/api/test/all’), @GetMapping(‘/api/test/[role]’)
repository has intefaces that extend Spring Data JPA JpaRepository to interact with Database.

UserRepository extends JpaRepository<User, Long>
RoleRepository extends JpaRepository<Role, Long>
models defines two main models for Authentication (User) & Authorization (Role). They have many-to-many relationship.

User: id, username, email, password, roles
Role: id, name
payload defines classes for Request and Response objects

We also have application.properties for configuring Spring Datasource, Spring Data JPA and App properties (such as JWT Secret string or Token expiration time).
