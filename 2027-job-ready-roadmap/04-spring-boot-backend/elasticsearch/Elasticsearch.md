# Elasticsearch – Important Topics & Learning Resources

## 1. Basics
- [ ] What is Elasticsearch and when to use it
- [ ] Documents, indices, shards and replicas
- [ ] Cluster, nodes and roles
- [ ] Running locally with Docker + Kibana
- [ ] REST API and Kibana Dev Tools

## 2. Search Internals
- [ ] Inverted index
- [ ] Analyzers, tokenizers and filters
- [ ] Mappings and field types (`text` vs `keyword`)
- [ ] Relevance scoring (BM25)
- [ ] Near-real-time search and refresh

## 3. Query DSL
- [ ] `match`, `match_phrase`, `multi_match`
- [ ] `term` and `terms`
- [ ] `range`
- [ ] `bool` (must, should, must_not, filter)
- [ ] Fuzzy search
- [ ] Highlighting
- [ ] Pagination (`from/size`, `search_after`)
- [ ] Aggregations (terms, range, date_histogram)

## 4. Spring Integration
- [ ] `spring-boot-starter-data-elasticsearch`
- [ ] `@Document`, `@Field`
- [ ] `ElasticsearchRepository`
- [ ] `ElasticsearchOperations` and `NativeQuery`
- [ ] Syncing data from the DB (dual write, CDC, batch reindex)

## 5. Production Topics
- [ ] Index aliases and zero-downtime reindexing
- [ ] Autocomplete (edge n-grams / completion suggester)
- [ ] Index lifecycle management
- [ ] Monitoring and cluster health

## 6. Practical Tasks
- [ ] Product search with filters, facets and fuzzy matching
- [ ] Autocomplete endpoint
- [ ] Sync a JPA entity to Elasticsearch on change

## Important Learning Resources
- [Elasticsearch Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Spring Data Elasticsearch Reference](https://docs.spring.io/spring-data/elasticsearch/reference/)
- [Baeldung – Elasticsearch with Spring Data](https://www.baeldung.com/spring-data-elasticsearch-tutorial)
- [Elastic Training / Free Courses](https://www.elastic.co/training/free)
