# Elasticsearch Full-Text Search & Analytics Engine

## 1. Introduction & Prerequisites
Elasticsearch is a distributed, JSON-based search and analytics engine built on Apache Lucene. It enables near-real-time full-text search, fuzzy matching, faceted navigation, and log analytics across massive distributed datasets where traditional relational database `LIKE '%pattern%'` queries fail to scale.

- **Prerequisites:** REST APIs, JSON, Basic SQL.
- **Estimated Study Time:** 14 hours
- **Difficulty:** Advanced

---

## 2. Detailed Learning Objectives
- Understand the Inverted Index data structure that powers Lucene search.
- Contrast Relational DB terms vs Elasticsearch concepts: Database -> Index, Row -> Document, Column -> Field.
- Master Elasticsearch Analysis: Character Filters, Tokenizers, and Token Filters (Stemming, Stop words).
- Construct complex Search Queries using Query DSL: `match`, `multi_match`, `term`, `bool` (must, should, filter, must_not).
- Integrate Elasticsearch with Spring Boot using Spring Data Elasticsearch.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Elasticsearch Cluster Architecture: Nodes, Master Node, Data Nodes, Shards & Replicas
- [ ] The Inverted Index: Term Dictionary, Postings List, Term Frequency (TF-IDF / BM25)
- [ ] Document CRUD via REST APIs (`POST /index/_doc/1`, `GET`, `PUT`, `DELETE`)
- [ ] Index Mappings: Explicit mappings, field data types (`text` vs `keyword`)
- [ ] Text Analysis Pipeline: Analyzers, Tokenizers (`standard`, `whitespace`), Filters (`lowercase`, `synonym`)
- [ ] Query DSL: Full-Text (`match`, `match_phrase`) vs Exact Value (`term`, `terms`, `range`)
- [ ] Compound Queries with `bool`: `must`, `filter` (cached, zero scoring), `should`, `must_not`
- [ ] Aggregations: Metric aggregations (avg, sum) and Bucket aggregations (terms, date_histogram)
- [ ] Fuzzy Search with Levenshtein Distance (`fuzziness: "AUTO"`)
- [ ] Spring Data Elasticsearch: `@Document`, `ElasticsearchRepository`

---

## 4. Free Documentation Resources
- **Elasticsearch Official Documentation:** [https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- **Baeldung — Spring Data Elasticsearch:** [https://www.baeldung.com/spring-data-elasticsearch-tutorial](https://www.baeldung.com/spring-data-elasticsearch-tutorial)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** *Elasticsearch Full Course for Beginners*
- **Java Brains:** *Elasticsearch Tutorial & Architecture*

---

## 6. Important Concepts Explained

### The Inverted Index Mechanism
Traditional databases index rows by ID. An Inverted Index parses text into tokens and maps each token to the list of document IDs that contain it.

```
Document 1: "Spring Boot is fast"
Document 2: "Spring Boot Microservices"

Inverted Index:
  Token           Postings List (Doc IDs)
  "boot"       -> [Doc 1, Doc 2]
  "fast"       -> [Doc 1]
  "microserv"  -> [Doc 2]
  "spring"     -> [Doc 1, Doc 2]
```

---

## 7. Practical Code Examples

### Spring Data Elasticsearch Document & Custom Query
```java
package com.roadmap.elasticsearch.domain;

import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;
import org.springframework.data.elasticsearch.annotations.Field;
import org.springframework.data.elasticsearch.annotations.FieldType;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;
import java.util.List;

@Document(indexName = "articles")
public class Article {
    @Id
    private String id;

    // 'text' type undergoes full-text analysis (tokenization, stemming)
    @Field(type = FieldType.Text, analyzer = "standard")
    private String title;

    // 'keyword' type is indexed as an exact string without tokenization (for filtering/sorting)
    @Field(type = FieldType.Keyword)
    private String category;
}

public interface ArticleRepository extends ElasticsearchRepository<Article, String> {
    // Custom method derived query
    List<Article> findByTitleContainingAndCategory(String keyword, String category);
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Set up an Elasticsearch Docker container and execute Query DSL requests to search a sample product catalog with fuzzy matching on misspelled search terms.
2. **Assignment 2:** Implement a multi-facet aggregation query that returns total product counts grouped by category and price buckets.

---

## 9. Mini-Projects
- **Full-Text Code & Document Search Engine:** Build a search engine microservice with Spring Data Elasticsearch that indexes Markdown files and provides highlighted search results with autocomplete suggestions.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between `text` and `keyword` field types in Elasticsearch?
**Answer:** `text` fields are analyzed: the incoming string is broken down into lowercase tokens through tokenizers and filters for full-text search (e.g. searching "quick" matches "The Quick Brown Fox"). `keyword` fields are NOT analyzed; they are stored as exact strings and used for exact matching, sorting, filtering, and aggregations (e.g. status codes, email addresses, tags).

### Q2: Why should you prefer `filter` over `must` in a `bool` query when scoring is not needed?
**Answer:** Queries inside the `must` clause contribute to the document's relevance score (`_score`), which consumes CPU cycles. Queries inside the `filter` clause only check a binary yes/no condition, do NOT calculate scoring, and are **automatically cached in memory** by Elasticsearch, resulting in significantly faster query execution.

---

## 11. Revision Notes
- Use `keyword` fields for aggregations and exact matching.
- Use `filter` clauses for non-scoring filters to exploit query caching.
- Primary shards cannot be modified after index creation; plan shard counts carefully.

---

## 12. Topic Completion Checklist
- [ ] Understand Inverted Index and BM25 scoring
- [ ] Differentiated `text` vs `keyword` fields
- [ ] Built complex `bool` queries with filters
- [ ] Integrated Spring Data Elasticsearch
