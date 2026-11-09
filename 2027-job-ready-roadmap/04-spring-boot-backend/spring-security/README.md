# Spring Security 6, JWT Authentication & OAuth2

## 1. Introduction & Prerequisites
Spring Security is the enterprise standard for authentication, authorization, and protection against common web vulnerabilities (CSRF, session fixation, clickjacking). Spring Security 6 (in Spring Boot 3) features a modernized component-based configuration architecture that completely abandons legacy adapter classes (`WebSecurityConfigurerAdapter`).

- **Prerequisites:** Spring Boot REST APIs, HTTP Headers, Cryptography basics (Hashing vs Encryption).
- **Estimated Study Time:** 22 hours
- **Difficulty:** Advanced

---

## 2. Detailed Learning Objectives
- Master the Spring Security Filter Chain architecture (`SecurityFilterChain`, `DelegatingFilterProxy`).
- Understand Authentication (Who are you?) vs Authorization (What can you do?).
- Implement stateless JWT (JSON Web Token) authentication: generation, signing (HMAC-SHA256/RSA), and verification.
- Implement Refresh Token rotation to allow secure long-lived sessions.
- Secure API endpoints using Role-Based Access Control (RBAC) and method security (`@PreAuthorize`).
- Protect against common web threats: CORS, CSRF, and timing attacks with `BCryptPasswordEncoder`.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Security Architecture: `SecurityContextHolder`, `SecurityContext`, `Authentication`
- [ ] Modern Component Configuration: Defining `SecurityFilterChain` bean
- [ ] Disabling CSRF for Stateless REST APIs and Configuring CORS
- [ ] UserDetails & UserDetailsService implementation
- [ ] Password Hashing: BCrypt and Argon2 algorithms
- [ ] Custom JWT Authentication Filter (`OncePerRequestFilter`)
- [ ] JWT Structure: Header, Payload (Claims), Signature
- [ ] Refresh Tokens: Rotation, Expiration, and Revocation in Redis
- [ ] Role-Based & Authority-Based Authorization (`hasRole('ADMIN')`, `hasAuthority('READ')`)
- [ ] Method Security: `@EnableMethodSecurity` and `@PreAuthorize("hasAuthority(...)")`

---

## 4. Free Documentation Resources
- **Spring Security 6 Official Documentation:** [https://docs.spring.io/spring-security/reference/](https://docs.spring.io/spring-security/reference/)
- **Baeldung — Spring Security Tutorials:** [https://www.baeldung.com/spring-security](https://www.baeldung.com/spring-security)
- **Auth0 — Introduction to JSON Web Tokens:** [https://jwt.io/introduction](https://jwt.io/introduction)

---

## 5. Free Video Courses & Tutorials
- **Amigoscode:** Spring Boot 3 Security & JWT Full Course
- **Java Brains:** Spring Security In Depth
- **Laurentiu Spilca:** Spring Security Architecture Deep Dive

---

## 6. Important Concepts Explained

### Spring Security Filter Chain Flow
Every incoming HTTP request passes through a sequence of filters before reaching `@RestController`:

```
Incoming Request
      │
      ▼
CorsFilter (Handles OPTIONS pre-flight & CORS headers)
      │
      ▼
CsrfFilter (Disabled for stateless JWT APIs)
      │
      ▼
JwtAuthenticationFilter (Extracts 'Bearer <token>', validates signature)
      │
      ├── Token Valid -> Sets Authentication into SecurityContextHolder
      │
      ▼
AuthorizationFilter (Verifies role/permissions: permits or throws 403 Forbidden)
      │
      ▼
DispatcherServlet -> Target @RestController
```

---

## 7. Practical Code Examples

### Modern Spring Security 6 `SecurityFilterChain` Configuration
```java
package com.roadmap.security.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity // Enables @PreAuthorize on methods
public class SecurityConfiguration {

    private final JwtAuthenticationFilter jwtFilter;

    public SecurityConfiguration(JwtAuthenticationFilter jwtFilter) {
        this.jwtFilter = jwtFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable()) // Stateless APIs do not require CSRF tokens
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/v1/auth/**", "/actuator/health").permitAll()
                .requestMatchers("/api/v1/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class)
            .build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a JWT generator utility that signs tokens with an HMAC-SHA256 secret key, encodes subject, issuance time, expiration, and user roles into claims.
2. **Assignment 2:** Implement a Refresh Token endpoint that rotates refresh tokens on every use and invalidates compromised tokens immediately.

---

## 9. Mini-Projects
- **Stateless RBAC Authentication Service:** Build a complete authentication microservice featuring user signup, login with BCrypt verification, access/refresh token generation, and secure endpoints protected by `@PreAuthorize("hasAuthority('SCOPE_write')")`.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why should JWT access tokens have a short expiration time (e.g., 15 minutes)?
**Answer:** Because JWTs are stateless and self-contained, a issued token cannot be easily invalidated by the server before its expiration timestamp without introducing state (such as a distributed blacklist in Redis). If an attacker intercepts a JWT, they can impersonate the user until it expires. Short-lived access tokens (10-15 mins) coupled with refresh tokens minimize the exposure window if a token is leaked.

### Q2: What is the difference between `hasRole()` and `hasAuthority()` in Spring Security?
**Answer:** `hasAuthority("ADMIN")` checks for an exact match against the granted authority string. `hasRole("ADMIN")` automatically prefixes the input with `ROLE_` (checking for `ROLE_ADMIN` internally).

---

## 11. Revision Notes
- Always hash passwords with `BCryptPasswordEncoder` (work factor 10 or 12).
- Never store sensitive data (passwords, social security numbers) inside JWT claims.
- Clear the `SecurityContextHolder` after request completion in custom filters.

---

## 12. Topic Completion Checklist
- [ ] Configured modern `SecurityFilterChain` bean
- [ ] Built custom `JwtAuthenticationFilter`
- [ ] Implemented Refresh Token rotation
- [ ] Secured controller methods using `@PreAuthorize`
