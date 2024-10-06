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


