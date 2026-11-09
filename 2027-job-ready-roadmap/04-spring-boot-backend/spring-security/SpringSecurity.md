# Spring Security 6 & JWT – Important Topics & Learning Resources

## 1. Security Fundamentals
- [ ] Authentication vs Authorization
- [ ] Common attacks: CSRF, XSS, SQL Injection, session fixation
- [ ] Password hashing: BCrypt, Argon2
- [ ] HTTPS / TLS basics
- [ ] OWASP Top 10

## 2. Spring Security Architecture
- [ ] Servlet filter chain
- [ ] `SecurityFilterChain` bean (no more `WebSecurityConfigurerAdapter`)
- [ ] `AuthenticationManager` and `AuthenticationProvider`
- [ ] `UserDetails` and `UserDetailsService`
- [ ] `PasswordEncoder`
- [ ] `SecurityContext` and `SecurityContextHolder`
- [ ] `authorizeHttpRequests` rules

## 3. Authorization
- [ ] Roles vs Authorities
- [ ] Role-Based Access Control (RBAC)
- [ ] Method security (`@EnableMethodSecurity`, `@PreAuthorize`, `@PostAuthorize`)
- [ ] SpEL in security expressions
- [ ] CORS and CSRF configuration

## 4. JWT Authentication
- [ ] JWT structure (header, payload, signature)
- [ ] Stateless vs session-based authentication
- [ ] Login endpoint issuing a token
- [ ] Custom JWT filter (`OncePerRequestFilter`)
- [ ] Access token + Refresh token flow
- [ ] Token expiry, revocation and blacklisting
- [ ] Signing algorithms (HS256 vs RS256)

## 5. OAuth2 & Advanced
- [ ] OAuth2 and OpenID Connect concepts
- [ ] Authorization Code flow with PKCE
- [ ] Spring Resource Server (`spring-boot-starter-oauth2-resource-server`)
- [ ] Social login (Google/GitHub)
- [ ] Keycloak integration
- [ ] Rate limiting and brute-force protection
- [ ] Security testing (`@WithMockUser`)

## 6. Practical Tasks
- [ ] Register/login API with BCrypt and JWT
- [ ] Add refresh tokens
- [ ] Protect endpoints by role (USER, ADMIN)
- [ ] Write tests for secured endpoints

## Important Learning Resources
- [Spring Security Reference](https://docs.spring.io/spring-security/reference/)
- [Baeldung – Spring Security](https://www.baeldung.com/security-spring)
- [jwt.io – Introduction](https://jwt.io/introduction)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Spring Security Samples (GitHub)](https://github.com/spring-projects/spring-security-samples)
