# MySQL使用过程笔记

- 在 MySQL 中，可以使用 `INSERT ... ON DUPLICATE KEY UPDATE` 语句来实现根据指定条件修改或新增字段

- 使用`SQL_CALC_FOUND_ROWS `让 MySQL 在计算结果集时考虑完整的数据行数，而不仅仅是 LIMIT 子句指定的行数。这样就可以在不重新执行查询的情况下，通过 FOUND_ROWS() 获取完整结果集的行数。

  - SQL_CALC_FOUND_ROWS:

  当你使用 LIMIT 子句限制查询结果时，SQL_CALC_FOUND_ROWS 指示 MySQL 在应用 LIMIT 之前计算结果集的总行数。
  这样，即使你只获取了部分结果（例如分页查询），你仍然可以知道整个结果集有多少行。

  - FOUND_ROWS():

  这个函数返回最近一次使用 SQL_CALC_FOUND_ROWS 的查询所产生的总行数。
  它不需要重新执行查询，因此提高了性能。
  举个例子：

  ```javascript
  const whereClause = "WHERE user_id = ? AND notes IS NOT NULL AND is_delete = 0 ";
  const dataQuery = `
    SELECT SQL_CALC_FOUND_ROWS word_id, subject_id, notes 
    FROM fllp_user_words 
    ${whereClause} 
    LIMIT ?, ?;
    SELECT FOUND_ROWS() AS total_count;
  `;
  
  connection.query(dataQuery, [user_id, offset, limit], (err, results) => {
    if (err) ...// 处理错误
    const data = results[0]; // 获取查询的数据部分
    const totalCount = results[1][0].total_count; // 获取总行数
  
    const response = {
  	total_count: totalCount,
  	data: data
    };
  });
  ```

  在这个例子中：

  `SQL_CALC_FOUND_ROWS`告诉 MySQL 计算完整结果集的行数，即使应用了 LIMIT。
  `SELECT FOUND_ROWS()` 作为第二个查询，返回第一个查询的完整结果集行数。
  `results[0]` 包含查询的数据部分，`results[1][0].total_count`包含总行数。
  这样做的好处是你可以在一次查询中获取到总记录数和实际的数据，而无需分别执行两个查询，从而提高性能并简化代码。
  
  

## 预加载

MySQL 数据库本身并没有一个专门的“预加载”机制，但可以通过一些技术和策略来实现类似的效果，以提高查询性能和减少延迟。以下是一些常见的方法：

1. **查询缓存（Query Cache）**

虽然 MySQL 8.0 及以上版本已经移除了查询缓存功能，但在 MySQL 5.7 及之前的版本中，查询缓存可以存储 SELECT 语句的结果，以便在相同的查询再次执行时直接返回缓存结果，而不需要重新执行查询。

2. **InnoDB 缓存池（InnoDB Buffer Pool）**

InnoDB 存储引擎有一个缓存池，用于缓存表数据和索引数据。当数据被频繁访问时，它们会被缓存到内存中，从而加快后续的查询速度。可以通过调整 `innodb_buffer_pool_size` 参数来增加缓存池的大小。

3. **预热缓存**

可以通过在应用程序启动时或在低峰时段执行一些常见的查询来预热缓存。这样，当实际用户请求到来时，数据已经在内存中，可以快速返回结果。

4. **分区表（Partitioning）**

将大表分成多个小表（分区），可以提高查询性能。每个分区可以独立地进行缓存和优化。

5. **使用内存表（Memory Tables）**

对于一些小的、读多写少的数据集，可以考虑使用 MEMORY 存储引擎。MEMORY 表的数据存储在内存中，因此查询速度非常快，但数据在服务器重启后会丢失。

6. **持久化缓冲池（InnoDB Persistent Memory Pool）**

从 MySQL 5.6 开始，InnoDB 支持持久化缓冲池，可以在重启后恢复缓存池的内容，从而减少冷启动时的性能损失。可以通过设置 `innodb_buffer_pool_load_at_startup` 和 `innodb_buffer_pool_dump_at_shutdown` 参数来启用此功能。

7. **预读取（Pre-fetching）**

在某些情况下，可以使用多线程或异步查询来预读取数据。例如，可以在用户登录时预读取用户经常访问的数据，以减少后续请求的延迟。

8. **使用缓存层（如 Redis 或 Memcached）**

虽然这不是 MySQL 本身的特性，但可以在 MySQL 之上使用缓存层来存储常用数据。当应用程序需要这些数据时，首先从缓存中读取，如果缓存中没有再从 MySQL 中读取并更新缓存。

9. **索引优化**

合理设计和使用索引可以显著提高查询性能。通过分析查询模式，可以创建适当的索引来加速数据检索。

10. **分区和分片（Sharding）**

对于非常大的数据集，可以考虑将数据分布在多个数据库实例上，每个实例处理一部分数据。这可以提高查询性能和可扩展性。



## 行锁定

行级锁定是一种数据库锁机制，它允许并发事务在同一张表上执行不同的操作，但对同一行数据的操作会被阻塞，直到锁定被释放。这种机制可以防止多个事务同时修改同一行数据，从而避免数据不一致的问题。

```sql
SELECT * FROM node_task 
WHERE `node_id` = 1 AND `app_id` = 1 AND `status` = 0 AND `exec_status` = 0 AND `deleted_at` IS NULL 
FOR UPDATE;
```

这条 SQL 语句会锁定满足条件的所有行，直到当前事务结束（提交或回滚）

**`FOR UPDATE` 锁的作用**:

- **锁定行**：当一个事务执行带有 `FOR UPDATE` 锁的查询时，它会锁定查询结果集中的所有行。其他事务在这些行上执行 `SELECT ... FOR UPDATE`、`UPDATE` 或 `DELETE` 操作时会被阻塞，直到第一个事务提交或回滚。
- **防止数据竞争**：这种锁定机制可以防止多个事务同时修改同一行数据，从而确保数据的一致性。
