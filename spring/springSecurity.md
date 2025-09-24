# Spring Security Complete Tutorial 🔐

## Table of Contents
1. [Introduction to Spring Security](#introduction)
2. [Why Spring Security?](#why-spring-security)
3. [Core Concepts](#core-concepts)
4. [Setting up Basic Spring Security](#basic-setup)
5. [Authentication vs Authorization](#auth-concepts)
6. [Password Encoding with BCrypt](#password-encoding)
7. [Database Integration](#database-integration)
8. [Advanced Features](#advanced-features)
9. [Interview Questions & Answers](#interview-questions)
10. [Best Practices](#best-practices)

## Introduction to Spring Security {#introduction}

**Spring Security** is a powerful and highly customizable authentication and authorization framework for Java applications, particularly Spring-based applications. It provides comprehensive security services and is the de-facto standard for securing Spring applications.

### Key Points:
- **Primary Purpose**: Secure Spring applications through authentication and authorization
- **Data Protection**: Critical for protecting sensitive user data and application resources
- **Password Management**: Provides robust mechanisms for secure password storage and validation
- **Built-in Features**: Login, logout, session management, CSRF protection, and more
- **Minimal Configuration**: Works out-of-the-box with sensible defaults

## Why Spring Security? {#why-spring-security}

### Security is Critical Because:
- **Data Protection**: User credentials, personal information, and business data need protection
- **Compliance**: Meet regulatory requirements (GDPR, HIPAA, etc.)
- **Trust**: Users must trust that their data is secure
- **Business Continuity**: Security breaches can destroy businesses

### Spring Security Advantages:
- **Comprehensive**: Handles both authentication and authorization
- **Flexible**: Highly customizable for different security requirements
- **Integration**: Seamlessly integrates with Spring ecosystem
- **Industry Standard**: Widely adopted and battle-tested
- **Active Development**: Regular updates and security patches

## Core Concepts {#core-concepts}

### 1. Authentication
**What it is**: Verifying "who you are"
- Username/password verification
- Token-based authentication
- Multi-factor authentication
- OAuth integration

### 2. Authorization
**What it is**: Determining "what you can do"
- Role-based access control (RBAC)
- Method-level security
- URL-based security
- Resource protection

### 3. Principal
The currently authenticated user or system

### 4. Security Context
Holds authentication information for the current thread

### 5. Security Filter Chain
Series of filters that process security concerns

## Basic Spring Boot Setup {#basic-setup}

### Step 1: Create Spring Boot Project

```xml
<!-- pom.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>spring-security-tutorial</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.0</version>
        <relativePath/>
    </parent>
    
    <dependencies>
        <!-- Spring Boot Web Starter -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- JSP Support - Tomcat Jasper -->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>
        
        <!-- JSTL for JSP -->
        <dependency>
            <groupId>org.glassfish.web</groupId>
            <artifactId>jakarta.servlet.jsp.jstl</artifactId>
        </dependency>
    </dependencies>
</project>
```

### Step 2: Create Controller

```java
// HomeController.java
@Controller
public class HomeController {
    
    @GetMapping("/")
    public String home() {
        return "home"; // Returns home.jsp
    }
    
    @GetMapping("/admin")
    public String admin() {
        return "admin"; // Returns admin.jsp
    }
}
```

### Step 3: Configure JSP View Resolver

```properties
# application.properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
server.port=8080
```

### Step 4: Create JSP Files

```jsp
<!-- src/main/webapp/WEB-INF/views/home.jsp -->
<!DOCTYPE html>
<html>
<head>
    <title>Home Page</title>
</head>
<body>
    <h1>Welcome to Home Page!</h1>
    <p>This is a public page accessible to everyone.</p>
    <a href="/admin">Go to Admin Page</a>
</body>
</html>
```

**At this point**: Visit `http://localhost:8080/` - you'll see the home page without any security.

## Adding Spring Security {#adding-security}

### Step 5: Add Spring Security Dependency

```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### What Happens Immediately:
1. **Automatic Security**: All endpoints are now secured
2. **Default Login Form**: Spring provides a built-in login page
3. **Default User**: Username is `user`
4. **Generated Password**: Check console logs for auto-generated password
5. **Session Management**: Automatic session handling
6. **CSRF Protection**: Enabled by default
7. **Logout Functionality**: Available at `/logout`

### Console Output Example:
```
Using generated security password: a1b2c3d4-e5f6-7890-abcd-ef1234567890
```

### Step 6: Test Basic Security
1. Go to `http://localhost:8080/`
2. You'll be redirected to `/login`
3. Enter credentials:
   - Username: `user`
   - Password: (from console)
4. After login, you'll see the home page

## Authentication vs Authorization {#auth-concepts}

### Authentication (Who are you?)

```java
// Custom Authentication Configuration
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user1 = User.builder()
            .username("john")
            .password(passwordEncoder().encode("password123"))
            .roles("USER")
            .build();
            
        UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder().encode("admin123"))
            .roles("ADMIN")
            .build();
            
        return new InMemoryUserDetailsManager(user1, admin);
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### Authorization (What can you do?)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/", "/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutSuccessUrl("/")
                .permitAll()
            );
            
        return http.build();
    }
}
```

## Password Encoding with BCrypt {#password-encoding}

### Why BCrypt?
- **Adaptive**: Can increase rounds as hardware improves
- **Salt**: Built-in salt prevents rainbow table attacks
- **Slow**: Intentionally slow to prevent brute force attacks
- **Industry Standard**: Widely accepted and secure

### BCrypt Implementation:

```java
@Service
public class UserService {
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    public void createUser(String username, String rawPassword) {
        // Encode password before storing
        String encodedPassword = passwordEncoder.encode(rawPassword);
        
        // Save to database
        User user = new User(username, encodedPassword);
        userRepository.save(user);
    }
    
    public boolean validatePassword(String rawPassword, String encodedPassword) {
        return passwordEncoder.matches(rawPassword, encodedPassword);
    }
}
```

### Password Encoding Configuration:

```java
@Configuration
public class PasswordConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12); // 12 rounds
    }
}
```

## Database Integration {#database-integration}

### Problem with Static Passwords:
- **Single User**: Only one user can access the system
- **No Persistence**: Password changes on restart
- **Not Scalable**: Cannot handle multiple users
- **No User Management**: Cannot add/remove users dynamically

### Solution: Database Integration

### Step 1: Add Database Dependencies

```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

### Step 2: Create User Entity

```java
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String password;
    
    @Column(nullable = false)
    private String email;
    
    @Enumerated(EnumType.STRING)
    private Role role;
    
    @Column(name = "account_non_expired")
    private boolean accountNonExpired = true;
    
    @Column(name = "account_non_locked")
    private boolean accountNonLocked = true;
    
    @Column(name = "credentials_non_expired")
    private boolean credentialsNonExpired = true;
    
    @Column(name = "enabled")
    private boolean enabled = true;
    
    // Constructors, getters, setters
}

enum Role {
    USER, ADMIN, MODERATOR
}
```

### Step 3: Create UserRepository

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    Optional<User> findByUsername(String username);
    
    Optional<User> findByEmail(String email);
    
    boolean existsByUsername(String username);
    
    boolean existsByEmail(String email);
}
```

### Step 4: Implement UserDetailsService

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    @Transactional
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
        
        return UserPrincipal.create(user);
    }
}
```

### Step 5: Create UserPrincipal

```java
public class UserPrincipal implements UserDetails {
    private Long id;
    private String username;
    private String email;
    private String password;
    private Collection<? extends GrantedAuthority> authorities;
    private boolean accountNonExpired;
    private boolean accountNonLocked;
    private boolean credentialsNonExpired;
    private boolean enabled;
    
    public UserPrincipal(Long id, String username, String email, String password,
                        Collection<? extends GrantedAuthority> authorities,
                        boolean accountNonExpired, boolean accountNonLocked,
                        boolean credentialsNonExpired, boolean enabled) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.password = password;
        this.authorities = authorities;
        this.accountNonExpired = accountNonExpired;
        this.accountNonLocked = accountNonLocked;
        this.credentialsNonExpired = credentialsNonExpired;
        this.enabled = enabled;
    }
    
    public static UserPrincipal create(User user) {
        List<GrantedAuthority> authorities = List.of(
            new SimpleGrantedAuthority("ROLE_" + user.getRole().name())
        );
        
        return new UserPrincipal(
            user.getId(),
            user.getUsername(),
            user.getEmail(),
            user.getPassword(),
            authorities,
            user.isAccountNonExpired(),
            user.isAccountNonLocked(),
            user.isCredentialsNonExpired(),
            user.isEnabled()
        );
    }
    
    // Implement all UserDetails methods
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }
    
    @Override
    public boolean isAccountNonExpired() {
        return accountNonExpired;
    }
    
    @Override
    public boolean isAccountNonLocked() {
        return accountNonLocked;
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        return credentialsNonExpired;
    }
    
    @Override
    public boolean isEnabled() {
        return enabled;
    }
    
    // Other getters
}
```

### Step 6: Update Security Configuration

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true)
public class SecurityConfig {
    
    @Autowired
    private CustomUserDetailsService userDetailsService;
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/", "/login", "/register", "/css/**", "/js/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard", true)
                .failureUrl("/login?error=true")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/")
                .invalidateHttpSession(true)
                .deleteCookies("JSESSIONID")
                .permitAll()
            )
            .userDetailsService(userDetailsService);
            
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12);
    }
    
    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authConfig) throws Exception {
        return authConfig.getAuthenticationManager();
    }
}
```

## Advanced Features {#advanced-features}

### 1. Method-Level Security

```java
@Service
public class DocumentService {
    
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteDocument(Long id) {
        // Only admins can delete
    }
    
    @PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
    public Document getDocument(Long id) {
        // Users and admins can view
    }
    
    @PostAuthorize("returnObject.owner.username == authentication.name")
    public Document getPersonalDocument(Long id) {
        // Users can only view their own documents
    }
}
```

### 2. Custom Login Page

```jsp
<!-- login.jsp -->
<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
    <style>
        .error { color: red; }
        .logout { color: green; }
    </style>
</head>
<body>
    <h2>Login Page</h2>
    
    <c:if test="${param.error != null}">
        <div class="error">Invalid username or password!</div>
    </c:if>
    
    <c:if test="${param.logout != null}">
        <div class="logout">You have been logged out successfully!</div>
    </c:if>
    
    <form action="/login" method="post">
        <div>
            <label>Username:</label>
            <input type="text" name="username" required>
        </div>
        <div>
            <label>Password:</label>
            <input type="password" name="password" required>
        </div>
        <div>
            <input type="checkbox" name="remember-me"> Remember Me
        </div>
        <div>
            <input type="submit" value="Login">
        </div>
        <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
    </form>
    
    <a href="/register">Don't have an account? Register here</a>
</body>
</html>
```

### 3. CSRF Protection

```java
// CSRF is enabled by default
@PostMapping("/transfer")
public String transfer(@RequestParam String amount, 
                      HttpServletRequest request) {
    // CSRF token is automatically validated
    return "transfer-success";
}
```

### 4. Session Management

```java
@Configuration
public class SessionConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .maximumSessions(1)
                .maxSessionsPreventsLogin(false)
                .sessionRegistry(sessionRegistry())
            );
            
        return http.build();
    }
    
    @Bean
    public SessionRegistry sessionRegistry() {
        return new SessionRegistryImpl();
    }
}
```

### 5. Remember Me Functionality

```java
@Configuration
public class RememberMeConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .rememberMe(remember -> remember
                .key("uniqueAndSecret")
                .tokenValiditySeconds(86400) // 24 hours
                .userDetailsService(userDetailsService)
            );
            
        return http.build();
    }
}
```

## Interview Questions & Answers {#interview-questions}

### Q1: What is Spring Security and why do we need it?
**Answer**: Spring Security is a comprehensive security framework for Java applications that provides authentication and authorization capabilities. We need it because:
- Data protection is critical in modern applications
- It provides industry-standard security practices out of the box
- Handles complex security scenarios like CSRF, session management, etc.
- Integrates seamlessly with Spring ecosystem

### Q2: Explain the difference between Authentication and Authorization.
**Answer**: 
- **Authentication**: Verifies "who you are" - validates user identity through credentials
- **Authorization**: Determines "what you can do" - controls access to resources based on user roles/permissions

### Q3: How does BCrypt work and why is it preferred?
**Answer**: BCrypt is an adaptive hashing function that:
- Uses built-in salt to prevent rainbow table attacks
- Allows configurable rounds to adjust computational cost
- Is intentionally slow to prevent brute force attacks
- Can adapt to hardware improvements by increasing rounds

### Q4: What happens when you add Spring Security dependency?
**Answer**: Immediately:
- All endpoints become secured
- Default login form is provided
- Default user 'user' is created with generated password
- Session management is enabled
- CSRF protection is activated
- Basic security headers are added

### Q5: Explain the Security Filter Chain.
**Answer**: A series of filters that process HTTP requests for security concerns:
1. SecurityContextPersistenceFilter - manages SecurityContext
2. UsernamePasswordAuthenticationFilter - handles form login
3. ExceptionTranslationFilter - handles security exceptions
4. FilterSecurityInterceptor - makes authorization decisions

### Q6: How do you implement custom authentication?
**Answer**: 
1. Implement UserDetailsService interface
2. Create custom UserDetails implementation
3. Configure AuthenticationProvider
4. Set up password encoding
5. Configure security filter chain

### Q7: What is SecurityContext and SecurityContextHolder?
**Answer**: 
- **SecurityContext**: Interface that holds Authentication object
- **SecurityContextHolder**: Utility class that associates SecurityContext with current thread
- Provides access to currently authenticated user information

### Q8: How do you handle CSRF in Spring Security?
**Answer**: 
- Enabled by default for state-changing operations (POST, PUT, DELETE)
- Uses synchronizer token pattern
- Can be disabled for REST APIs
- Tokens are automatically included in forms

### Q9: Explain method-level security.
**Answer**: Security applied at method level using annotations:
- @PreAuthorize - checks before method execution
- @PostAuthorize - checks after method execution  
- @Secured - simple role-based security
- @RolesAllowed - JSR-250 annotation

### Q10: How do you configure multiple authentication providers?
**Answer**:
```java
@Bean
public AuthenticationManager authManager(HttpSecurity http) throws Exception {
    return http.getSharedObject(AuthenticationManagerBuilder.class)
        .userDetailsService(userDetailsService)
        .passwordEncoder(passwordEncoder())
        .and()
        .ldapAuthentication()
        .userDnPatterns("uid={0},ou=people")
        .and()
        .build();
}
```

## Best Practices {#best-practices}

### 1. Password Security
- Always use BCrypt or similar adaptive hashing
- Never store passwords in plain text
- Implement password complexity requirements
- Use secure password reset mechanisms

### 2. Session Management
- Configure appropriate session timeout
- Implement session fixation protection
- Use HTTPS for all authentication-related operations
- Consider concurrent session control

### 3. Authorization
- Follow principle of least privilege
- Use role-based access control (RBAC)
- Implement defense in depth
- Regular security audits

### 4. CSRF Protection
- Keep CSRF protection enabled for web applications
- Use proper CSRF tokens
- Validate all state-changing operations
- Consider SameSite cookie attributes

### 5. HTTPS Configuration
```java
@Configuration
public class HttpsConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .requiresChannel(channel -> 
                channel.requestMatchers(r -> r.getHeader("X-Forwarded-Proto") != null)
                       .requiresSecure())
            .headers(headers -> headers
                .httpStrictTransportSecurity(hstsConfig -> hstsConfig
                    .maxAgeInSeconds(31536000)
                    .includeSubdomains(true)
                )
            );
            
        return http.build();
    }
}
```

### 6. Input Validation
```java
@RestController
public class UserController {
    
    @PostMapping("/users")
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDto userDto) {
        // @Valid triggers validation
        userService.createUser(userDto);
        return ResponseEntity.ok().build();
    }
}

public class UserDto {
    @NotBlank(message = "Username is required")
    @Size(min = 3, max = 20)
    private String username;
    
    @NotBlank(message = "Password is required")
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;
    
    @Email(message = "Email should be valid")
    private String email;
}
```

### 7. Logging and Monitoring
```java
@Component
public class SecurityEventListener {
    
    private static final Logger logger = LoggerFactory.getLogger(SecurityEventListener.class);
    
    @EventListener
    public void handleAuthenticationSuccess(AuthenticationSuccessEvent event) {
        logger.info("Authentication successful for user: {}", 
                   event.getAuthentication().getName());
    }
    
    @EventListener
    public void handleAuthenticationFailure(AbstractAuthenticationFailureEvent event) {
        logger.warn("Authentication failed for user: {}", 
                   event.getAuthentication().getName());
    }
}
```

## Conclusion

Spring Security is a powerful framework that provides comprehensive security features for Java applications. By understanding its core concepts, proper implementation patterns, and best practices, you can build secure applications that protect user data and maintain system integrity.

Remember: Security is not a one-time implementation but an ongoing process that requires regular updates, monitoring, and improvement.

---

## Additional Resources

- [Spring Security Documentation](https://spring.io/projects/spring-security)
- [OWASP Security Guidelines](https://owasp.org/)
- [Spring Boot Security Configuration](https://spring.io/guides/gs/securing-web/)

## Contributing

Feel free to contribute to this tutorial by submitting pull requests or opening issues for improvements and corrections.

## License

This tutorial is provided under the MIT License. Feel free to use and modify as needed.
