# Spring Core Framework, IoC Container & Dependency Injection

## 1. Introduction & Prerequisites
The Spring Framework is built on Inversion of Control (IoC) and Dependency Injection (DI). By transferring object lifecycle and dependency assembly from application code to the Spring `ApplicationContext`, applications become loosely coupled, highly testable, and modular.

- **Prerequisites:** Java OOP, Interfaces, Annotations.
- **Estimated Study Time:** 14 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Understand Inversion of Control (IoC) vs Dependency Injection (DI).
- Master Spring Bean Lifecycles: Instantiation, Population, Aware interfaces, Post-Processors, `@PostConstruct`, and `@PreDestroy`.
- Master Bean Scopes: Singleton, Prototype, Request, Session, Application.
- Contrast Constructor Injection vs Setter Injection vs Field Injection (why field injection is an anti-pattern).
- Master Spring Aspect-Oriented Programming (AOP): Pointcuts, Joinpoints, Advices (`@Before`, `@AfterReturning`, `@Around`).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Spring Container: `BeanFactory` vs `ApplicationContext`
- [ ] Stereotype Annotations: `@Component`, `@Service`, `@Repository`, `@Controller`, `@Configuration`
- [ ] `@Bean` methods inside `@Configuration` classes
- [ ] Constructor Injection vs Field Injection (`@Autowired`)
- [ ] Resolving Ambiguity: `@Primary` and `@Qualifier`
- [ ] Bean Scopes: Singleton vs Prototype behavior
- [ ] Bean Lifecycle Hooks: `BeanPostProcessor`, `@PostConstruct`, `@PreDestroy`
- [ ] Lazy Initialization (`@Lazy`) and Circular Dependencies
- [ ] Spring Profiles (`@Profile("dev")`, `@Profile("prod")`)
- [ ] Spring AOP: Cross-cutting concerns, Proxies (JDK Dynamic vs CGLIB), and `@Around` advice

---

## 4. Free Documentation Resources
- **Spring Framework Reference — Core Technologies:** [https://docs.spring.io/spring-framework/reference/core.html](https://docs.spring.io/spring-framework/reference/core.html)
- **Baeldung — Spring Core Tutorials:** [https://www.baeldung.com/spring-core](https://www.baeldung.com/spring-core)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Spring Framework In Depth
- **Amigoscode:** Spring Framework Tutorial
- **Marco Codes:** Spring Beans & Dependency Injection Explained

---

## 6. Important Concepts Explained

### Bean Lifecycle Flow in Spring ApplicationContext
```
Class Bytecode
      │
      ▼
Instantiate Bean (Constructor)
      │
      ▼
Populate Properties (Dependency Injection)
      │
      ▼
BeanNameAware / ApplicationContextAware Callbacks
      │
      ▼
BeanPostProcessor: postProcessBeforeInitialization()
      │
      ▼
@PostConstruct / InitializingBean.afterPropertiesSet()
      │
      ▼
BeanPostProcessor: postProcessAfterInitialization() (AOP Proxies created here!)
      │
      ▼
Bean is Ready for Use (Singleton Cache)
      │
      ▼ (Application Shutdown)
@PreDestroy / DisposableBean.destroy()
```

---

## 7. Practical Code Examples

### Clean Constructor Injection & Custom Performance Logging AOP Aspect
```java
package com.roadmap.springcore;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

// Clean Service with Constructor Injection (No @Autowired annotation needed)
@Service
public class OrderService {
    private final NotificationPort notificationPort;

    public OrderService(NotificationPort notificationPort) {
        this.notificationPort = notificationPort;
    }

    public void processOrder(String orderId) {
        // Business logic...
        notificationPort.send("Order processed: " + orderId);
    }
}

// AOP Aspect for Execution Timing
@Aspect
@Component
public class PerformanceMonitoringAspect {

    @Around("execution(* com.roadmap.springcore.OrderService.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        try {
            return joinPoint.proceed();
        } finally {
            long duration = System.currentTimeMillis() - start;
            System.out.printf("Method [%s] executed in %d ms%n", 
                joinPoint.getSignature().toShortString(), duration);
        }
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Create two implementations of a `PaymentGateway` interface (`StripeGateway`, `PayPalGateway`) and inject them selectively using `@Qualifier` and Spring Profiles.
2. **Assignment 2:** Implement a custom `BeanPostProcessor` that inspects beans for a custom annotation `@Audited` and wraps them in an auditing proxy.

---

## 9. Mini-Projects
- **Aspect-Driven Audit Logging Framework:** Build an AOP-based logging library that automatically logs user ID, execution timestamp, arguments, and return values for all methods annotated with `@LogExecution`.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why is field injection (`@Autowired private UserService userService;`) considered an anti-pattern?
**Answer:**
1. **Violates Immutability:** Fields cannot be declared `final`, making the class prone to unwanted mutations.
2. **Impairs Testability:** Unit tests cannot instantiate the class and inject mock dependencies without reflection or starting a heavyweight Spring container.
3. **Hides Dependencies:** It hides architectural complexity; a class can easily accumulate 10 injected fields without looking cluttered. With constructor injection, a large constructor immediately signals an SRP violation.
4. **Prone to NPEs:** If instantiated manually outside Spring, injected fields remain `null`.

### Q2: What is the difference between JDK Dynamic Proxy and CGLIB Proxy in Spring?
**Answer:** Spring AOP creates proxies around beans to implement aspects (transactions, security, logging). If the target class implements at least one interface, Spring uses **JDK Dynamic Proxy** (which generates an implementation of that interface). If the target class does not implement any interface, Spring uses **CGLIB** (Code Generation Library) to generate a subclass of the target class at runtime. (In Spring Boot 2.x and 3.x, CGLIB is the default even when interfaces exist, for consistency).

---

## 11. Revision Notes
- Always prefer constructor injection with `final` fields.
- Singleton beans must be stateless to guarantee thread safety.
- `@Configuration` classes are proxied with CGLIB to ensure `@Bean` methods return singleton instances.

---

## 12. Topic Completion Checklist
- [ ] Understand IoC and Dependency Injection
- [ ] Traced complete Bean Lifecycle steps
- [ ] Replaced Field Injection with Constructor Injection
- [ ] Created custom AOP `@Around` advices
