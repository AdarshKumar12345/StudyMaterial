# Hibernate & JPA – Important Topics & Learning Resources

## 1. ORM & JPA Basics
- [ ] What is ORM and the object-relational mismatch
- [ ] JPA vs Hibernate vs Spring Data JPA
- [ ] DataSource and HikariCP connection pool
- [ ] `application.yml` datasource and `ddl-auto` options
- [ ] Flyway / Liquibase migrations

## 2. Entities & Mapping
- [ ] `@Entity`, `@Table`, `@Id`
- [ ] ID generation strategies (IDENTITY, SEQUENCE, UUID)
- [ ] `@Column`, `@Enumerated`, `@Lob`, `@Transient`
- [ ] `@Embedded` / `@Embeddable`
- [ ] `equals()` / `hashCode()` for entities
- [ ] Inheritance mapping strategies

## 3. Relationships
- [ ] `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`
- [ ] Owning side and `mappedBy`
- [ ] Unidirectional vs Bidirectional
- [ ] Cascade types and `orphanRemoval`
- [ ] Fetch types: LAZY vs EAGER
- [ ] `LazyInitializationException`

## 4. Spring Data JPA
- [ ] `JpaRepository`, `CrudRepository`
- [ ] Derived query methods
- [ ] `@Query` (JPQL and native)
- [ ] Pagination and Sorting
- [ ] Projections (interface and DTO)
- [ ] Specifications / Criteria API
- [ ] `@Modifying` queries

## 5. Performance & Transactions
- [ ] N+1 query problem
- [ ] `JOIN FETCH` and `@EntityGraph`
- [ ] Batch fetching and batch inserts
- [ ] `@Transactional` (propagation, isolation, readOnly, rollbackFor)
- [ ] Persistence context and entity states
- [ ] First-level and second-level cache
- [ ] Optimistic (`@Version`) vs pessimistic locking
- [ ] Auditing (`@CreatedDate`, `@LastModifiedBy`)
- [ ] Open-Session-in-View and why to disable it

## 6. Practical Tasks
- [ ] Model Users, Orders and Products with relationships
- [ ] Reproduce the N+1 problem and fix it with `@EntityGraph`
- [ ] Add optimistic locking and trigger a conflict
- [ ] Add Flyway migrations

## Important Learning Resources
- [Hibernate ORM Documentation](https://hibernate.org/orm/documentation/)
- [Spring Data JPA Reference](https://docs.spring.io/spring-data/jpa/reference/)
- [Baeldung – Persistence with Spring](https://www.baeldung.com/persistence-with-spring-series)
- [Vlad Mihalcea – Blog](https://vladmihalcea.com/)
- [Thorben Janssen – Thoughts on Java](https://thorben-janssen.com/)
