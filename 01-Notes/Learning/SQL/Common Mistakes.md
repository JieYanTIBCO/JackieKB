rank() **over** (partition by <> order by <>) as <>,
round(number, **digits**)
case
	**when condition A then <>**
	**when condition B then <>**
**end** as <>


**Select and Having** should only show group by and aggregation in Group by
在 `GROUP BY` 查询中，`SELECT` 中的列只能是以下两种类型之一：
- **分组列**：在 `GROUP BY` 中明确列出的字段。
- **聚合函数**：如 `SUM()`, `COUNT()`, `AVG()`, `MAX()`, `MIN()` 等。


`WHERE` 子句**不能直接使用聚合函数**（如 `SUM()`, `COUNT()` 等）。

- 聚合函数的过滤必须通过 `HAVING` 子句实现。
- `WHERE` 仅用于过滤分组前的原始数据。

where extract(year **from** or.order_date) **=** 2023