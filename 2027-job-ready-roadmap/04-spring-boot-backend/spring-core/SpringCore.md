# Spring Core & IoC – Important Topics & Learning Resources

## 1. Spring Fundamentals
- [ ] What is Spring Framework and its modules
- [ ] Inversion of Control (IoC)
- [ ] Dependency Injection (DI)
- [ ] Tight vs Loose Coupling
- [ ] Spring Container: BeanFactory vs ApplicationContext

## 2. Beans & Configuration
- [ ] What is a Bean
- [ ] XML vs Java Config vs Annotation Config
- [ ] `@Configuration` and `@Bean`
- [ ] Stereotypes: `@Component`, `@Service`, `@Repository`, `@Controller`
- [ ] `@ComponentScan`
- [ ] `@Primary` and `@Qualifier`
- [ ] `@Value` and `@PropertySource`
- [ ] `@Profile`

## 3. Dependency Injection
- [ ] Constructor Injection (recommended)
- [ ] Setter Injection
- [ ] Field Injection and why to avoid it
- [ ] `@Autowired` and resolving ambiguity
- [ ] Injecting collections, `Optional` and `ObjectProvider`
- [ ] Circular dependencies

## 4. Bean Lifecycle & Scopes
- [ ] Bean lifecycle phases
- [ ] `@PostConstruct` and `@PreDestroy`
- [ ] `InitializingBean` / `DisposableBean`
- [ ] `BeanPostProcessor`
- [ ] Scopes: singleton, prototype, request, session
- [ ] Singleton vs Prototype injection problem
- [ ] Lazy initialization (`@Lazy`)

## 5. AOP (Aspect-Oriented Programming)
- [ ] Cross-cutting concerns
- [ ] Aspect, Advice, Pointcut, JoinPoint
- [ ] `@Before`, `@After`, `@Around`, `@AfterReturning`, `@AfterThrowing`
- [ ] Pointcut expressions
- [ ] Proxies: JDK dynamic vs CGLIB
- [ ] Self-invocation pitfall

## 6. Other Core Topics
- [ ] Spring Events (`ApplicationEventPublisher`, `@EventListener`)
- [ ] SpEL (Spring Expression Language)
- [ ] Environment and PropertySources
- [ ] Resource loading

## 7. Practical Tasks
- [ ] Build a small app using only Spring Core (no Boot)
- [ ] Write a logging/timing aspect with `@Around`
- [ ] Demonstrate singleton vs prototype behaviour
- [ ] Reproduce and fix a circular dependency

## Important Learning Resources
- [Spring Framework Reference Documentation](https://docs.spring.io/spring-framework/reference/)
- [Spring Guides](https://spring.io/guides)
- [Baeldung – Spring Tutorial](https://www.baeldung.com/spring-tutorial)
- [Baeldung – Spring AOP](https://www.baeldung.com/spring-aop)
