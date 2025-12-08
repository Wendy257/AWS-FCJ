---
title: "Blog 3"
 
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# SQL to NoSQL: Planning your application migration to Amazon DynamoDB

Organizations migrating from relational databases to Amazon DynamoDB often face challenges in redesigning their data models and access patterns. The shift requires understanding fundamental differences in query capabilities, data modeling approaches, and application architecture is essential to achieve optimal performance while maintaining application functionality.

A real-world example illustrates these challenges: A social media platform experiencing rapid growth finds their relational database struggling with complex queries involving joins between multiple tables, particularly for features like trending posts and celebrity content interactions. The platform’s user base is projected to grow multi-fold soon, raising concerns about the existing architecture’s ability to scale. During viral events with thousands of concurrent users, complex join queries for trending content show signs of stress, indicating that the current architecture might not meet the platform’s growing scalability needs.

This is the first part of a series exploring how to effectively migrate from SQL to DynamoDB. We will examine how to analyze existing database structures and access patterns to prepare for migration, focusing on schema analysis, query patterns, and usage metrics that inform DynamoDB data model design. Subsequent posts will cover data modeling strategies and data access layer modernization.

---

## Relational compared to NoSQL databases

When transitioning your application from a traditional relational database to DynamoDB, there are several key differences compared to migrating between relational databases. Understanding these distinctions (detailed in the following table) is critical for a successful modernization.

| Aspect | Relational Databases | Amazon DynamoDB |
| :--- | :--- | :--- |
| **Query Language** | Uses SQL with structured query patterns | Provides API-based operations with consistent single-digit millisecond latency through optimized partition and sort key access |
| **Data Model** | Uses well-established normalized data structures | Offers flexible denormalized data modeling optimized for performance and scalability at any scale |
| **Data Access Framework** | Uses mature ORM frameworks like Entity Framework and Hibernate | Provides comprehensive native SDKs across multiple programming languages, with purpose-built libraries that enable high performance while maintaining familiar object-oriented patterns |

Migrating from one relational database to another relational database typically has a relatively low impact on the data access layer, because the fundamental structure and access patterns remain similar. Migrating to DynamoDB introduces a different approach that takes advantage of its powerful NoSQL capabilities. The shift in data model, query language, and access patterns enables applications to take advantage of DynamoDB high performance and scalability. Teams can adapt their data access layer to align with DynamoDB design principles, optimizing for consistent performance and efficiency.

Additionally, in a traditional relational database migration, the database team primarily handles the database setup, but the application team’s involvement is often limited. But when moving to DynamoDB, the collaboration between the application team and the database team becomes important. The application team must work closely with the database experts to design an optimal DynamoDB data model that aligns with the application’s data access requirements. This collaborative effort is key to a successful migration to the DynamoDB NoSQL database.

---

## Analysis for data modeling

Migrating from a relational database to DynamoDB benefits from careful planning to achieve optimal performance and cost-efficiency. First, we assess the existing relational database’s complexity to define our requirements. This evaluation guides the design of an appropriate DynamoDB data model that scales effectively with your workload. A thorough initial assessment provides a smoother transition to DynamoDB. The migration process involves analyzing current data structures, identifying access patterns, and optimizing for the DynamoDB key-value pair model.

### Social media platform use case

To demonstrate the concepts throughout this post, we use a social media platform where users can create and interact with posts. As shown in the following diagram, the core entities include users who create posts, with associated comments and likes for engagement. The platform tracks metrics through user counter and post counter entities, which maintain statistics like total posts per user and engagement metrics per post. The diagram demonstrates key relationships in our current relational model:

- Users to posts (one-to-many relationship)
- Posts to comments (one-to-many relationship)
- Users and posts to their respective counters (one-to-one relationship)
- Users to posts through likes (many-to-many relationship), where users can like multiple posts and posts can receive likes from multiple users

---

### Schema analysis

Begin with a thorough examination of your existing database structure. This analysis helps determine which tables to migrate and understand the complexity of data modeling in DynamoDB.

#### Database structure analysis

First, examine your current database architecture:

- Identify tables (transactional, analytical, audit logs, and so on) and their relationships
- Analyze data types (text, numbers, datetime, guid, spatial, and so on), and identify equivalent data types in DynamoDB
- Document table sizes and growth patterns, keeping in mind DynamoDB 400 KB per-item size limit

Understanding how your application uses different data types guides their representation in DynamoDB. Datetime fields need mapping to either ISO strings or epoch numbers based on your query and sort requirements. TEXT fields require size evaluation when denormalization combines multiple fields into a single item, such as post content with embedded comments. This analysis helps in designing a data model that supports efficient querying while maintaining data integrity.

#### Microservices considerations

If modernizing with a microservices architecture, identify table dependencies for each service:

- List tables required per service
- Document cross-service data dependencies
- Plan a denormalization strategy
- Evaluate data consistency requirements

Consider how services need to share and access data across boundaries. For example, the post service might require user names and profile pictures for display, whereas the notification service might need user preferences for alert delivery. These cross-service data dependencies influence your DynamoDB modeling decisions. Evaluate the trade-offs between maintaining strict service isolation vs. denormalizing specific attributes based on access patterns and update frequencies. Consider the performance impact of cross-service calls and plan for scenarios where service boundaries might need adjustment as application patterns evolve.

#### Partial migration analysis

When migrating only a subset of tables to DynamoDB, understand the relationships between migrated and non-migrated tables:

- Identify migration scope and dependencies
- Assess performance implications
- Plan application modifications
- Design data consistency strategies
- Define future migration phases

Consider a scenario where frequently accessed data like posts and comments move to DynamoDB while historical analytics data remains in the relational database. Design patterns to handle operations that previously relied on transactional consistency across tables. Plan for potential latency impacts of cross-database queries and implement appropriate data synchronization mechanisms. This understanding can help you develop effective strategies for maintaining data consistency and application performance during and after the migration.

#### Schema object evaluation

Evaluate how to implement SQL schema object functionalities in your DynamoDB design:

- Document SQL views and their application usage
- Analyze stored procedure complexity, including Common Table Expressions (CTEs)
- Plan the migration of trigger-based business logic
- Evaluate references to tables not part of migration
- Consider alternatives for complex analytical queries
- Plan for large-scale data modifications

SQL views that aggregate engagement metrics across tables, stored procedures calculating trending content with time-windowed data, and triggers maintaining derived data like post counts need reimagining for DynamoDB. Design your data models to support efficient access patterns without relying on SQL operations. Consider batch processing approaches for large-scale data modifications and plan how application logic will handle operations previously managed by database procedures and triggers.By conducting thorough schema analysis and planning carefully, teams can anticipate implementation requirements, optimize query patterns, and develop effective data consistency strategies. This understanding is key to a successful modernization effort, enabling teams to fully take advantage of DynamoDB capabilities in their applications.

---

### Query analysis

Analyze the SQL queries used in the application, including inline SQL, ORM-generated queries, and other dynamic queries. This analysis informs the selection of appropriate partition keys, sort keys, and relationships for the DynamoDB data model. Examine each component of the SQL queries as follows.

#### SELECT clause analysis

Examining SELECT clauses provides insights into how your application consumes data and the relationships between different entities. Consider these key aspects while analyzing SELECT clauses:

- Subqueries – Look for subqueries in the existing access patterns that might typically have aggregations, filtering based on complex conditions, fetching related entity details, or retrieving latest records. For instance, a user profile query might use subqueries to calculate total posts and follower counts, whereas a post query might fetch the most recent comments. Understanding these patterns helps identify what data to denormalize and which attributes to maintain as pre-calculated values in DynamoDB.

```sql
-- User profile with total posts count
SELECT u.name, u.profile_pic,
(SELECT COUNT(*) FROM posts WHERE user_id = u.user_id) as post_count
FROM users u
WHERE u.user_id = '123'
```

- Multi-table column selection – Analyze how your queries combine columns from multiple tables, which indicates the data frequently accessed together. A typical post display query combines content with author details and engagement statistics in a single fetch. This analysis helps determine which attributes from different entities can be denormalized into a single item to support efficient read operations in DynamoDB.
  
```sql
-- Post with author details
SELECT p.content, p.created_at, u.name, u.profile_pic
FROM posts p
JOIN users u ON p.user_id = u.user_id
WHERE p.post_id = '456'
```

- User-defined functions – Review the usage of custom functions in your queries that transform or compute data during retrieval. Functions calculating trending scores by combining multiple engagement factors with time-based weights represent complex computations in queries. Understanding these computations helps identify attributes that should be pre-calculated and stored, and how to structure your data model to support efficient updates of these computed values in DynamoDB.
- Derived columns – Examine calculated or conditional columns in your queries that combine multiple field values or apply business logic. Queries often derive total engagement metrics and determine content status based on interaction thresholds. This analysis guides decisions about maintaining computed attributes in your items and designing efficient update patterns in DynamoDB to keep these derived values current.

```sql
-- Derived engagement score and post status
SELECT post_id, content,
(like_count + comment_count) as total_engagement,
CASE WHEN like_count > 1000 THEN 'TRENDING'
WHEN like_count > 100 THEN 'POPULAR'
ELSE 'NORMAL'
END as post_status
FROM posts
WHERE created_at > DATEADD(year, -1, GETUTCDATE())
```

#### JOIN clause analysis

Examining JOIN clauses in your SQL queries reveals the relationships between different entities and how your application combines data from multiple tables. This analysis helps determine the optimal data model strategy in DynamoDB. Consider these key aspects while analyzing JOIN clauses:

- Entity relationships – Analyze which tables are frequently joined together in your queries. A post display query might combine data from posts, comments, likes, and user tables, indicating these entities are commonly accessed together for this operation. Understanding these combinations helps identify what data might need to be denormalized or modeled together in DynamoDB for efficient access.
- Join conditions – Evaluate the conditions and cardinality of relationships between tables. For example, a post might join with multiple comments (one-to-many) while joining with exactly one user profile (one-to-one). Join conditions might also involve date ranges or status flags besides simple key matching. Understanding these cardinalities helps determine appropriate modeling strategies—whether to embed data within items or maintain separate items—and helps estimate read/write patterns for evaluating performance and cost implications of different modeling approaches.
  
```sql
-- Post with its comments
SELECT p.*, u.*, c.comment_text, c.created_at
FROM posts p
JOIN comments c ON p.post_id = c.post_id
AND c.created_at > DATEADD(hour, -24, GETUTCDATE())
JOIN users u ON p.user_id = u.user_id
WHERE p.post_id = '456'
```

- Cross-service and hybrid architecture joins – Review joins involving tables that belong to different services or tables not planned for migration. For instance, if post data joins with user profile data managed by a separate service, or with analytics data remaining in the relational database, these patterns inform your data modeling decisions. This analysis guides data access strategies across service boundaries and different databases.

#### WHERE clause analysis

Examining WHERE clauses in your SQL queries provides insights into how your application filters and accesses data. This analysis is valuable for designing efficient access patterns in DynamoDB. Consider these aspects while analyzing WHERE clauses:

- Single-value filters – Identify frequently used single-value filters in your queries. For example, filtering posts by a specific user ID or retrieving posts by status. These patterns help identify potential partition keys for your base table or global secondary indexes (GSIs) in DynamoDB, enabling efficient data retrieval across different access patterns.

```sql
-- Single value filter
SELECT * FROM posts WHERE user_id = '123'
AND status = 'ACTIVE'
```

- Range-based filters – Look for queries that combine entity identifiers with range conditions. A query filtering posts by user ID and date range indicates how to structure composite keys in DynamoDB. Understanding these patterns helps determine appropriate sort keys that support efficient range queries within a partition.

```sql
-- Getting user's recent posts
SELECT * FROM posts
WHERE user_id = '123'
AND created_at > DATEADD(year, -1, GETUTCDATE())
ORDER BY created_at DESC
```

- Multi-valued filters – Examine conditions involving multiple possible values for an attribute, like finding posts by a group of user IDs or posts with specific hashtags. These patterns influence your DynamoDB data modeling decisions. For example, retrieving posts by multiple users might require separate queries using a GSI with the user ID as partition key, whereas finding posts by hashtags might benefit from an inverted index structure where the hashtag serves as partition key with associated post IDs stored as a set.

```sql
-- Finding posts with specific hashtags
SELECT DISTINCT p.*
FROM posts p
JOIN post_hashtags ph ON p.post_id = ph.post_id
WHERE ph.hashtag IN ('#POPULAR', '#TRENDING')
```

- Cross-entity filters – Identify filters that span multiple entities in your SQL queries. For example, a query that finds posts based on both post attributes and user attributes (like user location or user type) indicates relationships that influence denormalization decisions in your DynamoDB model. When these filters involve entities managed by different microservices or entities not being migrated to DynamoDB, analyze the performance impact of client-side filtering and data retrieval across service boundaries.

```sql
-- Finding active user posts with high engagement
SELECT p.post_id, p.content,
FROM posts p
JOIN users u ON p.user_id = u.user_id
WHERE u.status = 'ACTIVE'
AND p.like_count > 100
ORDER BY p.created_at DESC
```

#### ORDER BY and TOP/LIMIT clause analysis

Examining ORDER BY and TOP/LIMIT clauses reveals data sorting and pagination requirements in your application. This analysis helps determine effective sort key design in DynamoDB. Consider these aspects:

- Sort requirements – Identify attributes and combinations of attributes used for ordering data. For example, sorting posts by creation date for displaying recent posts, or by engagement metrics for trending content. Some queries might need compound sorting, like ordering posts first by date and then by likes within each date. These patterns help determine appropriate sort key structures in DynamoDB that support your required sorting capabilities.
- Pagination requirements – Analyze queries using TOP/LIMIT clauses along with sorting. A query fetching the most recent posts with a limit, or top trending posts based on engagement, indicates pagination needs. Understanding these patterns helps design efficient pagination strategies using DynamoDB limit and LastEvaluatedKey features.

#### GROUP BY clause analysis

Examining GROUP BY operations reveals aggregation requirements in your application. This analysis helps identify metrics and counters that need to be maintained in your DynamoDB data model. Consider these aspects:

- Aggregation patterns – Identify the grouping and aggregation operations in your queries. For example, counting posts per user, calculating total likes per post, or summarizing engagement metrics by post type. Understanding these patterns helps determine which attributes need to be maintained as pre-calculated values in your items.

```sql
-- Posts count by user
SELECT user_id, COUNT(*) as total_posts
FROM posts
GROUP BY user_id
```

- Update frequency – Analyze how often these aggregated values change. A post’s like count updates frequently as users interact, whereas a user’s total post count changes only when they create or delete posts. These patterns help assess the write patterns and their impact on maintaining pre-calculated values.
  
#### Application usage analysis

Collect application usage statistics, including the frequency of HTTP requests per hour. Analyze both business importance and usage metrics with the following objectives:

- Prioritize edge cases that require special attention. For example:

    + When a celebrity posts an update, the system might have to deliver that post to millions of followers’ feeds instantly.
    + Viral posts might receive sudden surge of likes and comments within minutes.
  
- Identify key modules for migration.

This data-driven approach means that the most critical and frequently used parts of the application are given precedence during the migration process, optimizing resource allocation and minimizing potential disruptions to core business functions.Analyze the average row size, read-to-write ratio, and projected growth rate of tables with the following goals:

- Determine the optimal approach for building entity relationships, for example, choosing between single-item strategies or vertical partitioning
- Accurately estimate and allocate read and write capacity

## Conclusion

Through analysis of your current database structure, access patterns, and usage metrics, you can build a solid foundation for your DynamoDB migration. This understanding helps shape the next phase—designing your DynamoDB data model, which we will explore in Part 2 of this series. Following this structured approach helps make sure your migration strategy aligns with both your current needs and future scalability requirements.