# SQL Round (with working queries)

> **Time needed:** about 2.5 hours · **Questions:** 17
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.
> Better still: type each query yourself into a real database (PostgreSQL or MySQL) and run it.

## Progress checklist
- [ ] 1. The order SQL really runs in
- [ ] 2. JOIN types
- [ ] 3. Second-highest salary
- [ ] 4. ROW_NUMBER vs RANK vs DENSE_RANK
- [ ] 5. Highest-paid employee in each department
- [ ] 6. Find and delete duplicate emails
- [ ] 7. Customers who never ordered
- [ ] 8. Total spent per customer (top 10)
- [ ] 9. Running total and 7-day moving average
- [ ] 10. Month-over-month revenue growth
- [ ] 11. Top 3 products per category
- [ ] 12. Employees who earn more than their manager
- [ ] 13. Orders per day by status
- [ ] 14. Repeat customer rate
- [ ] 15. Products never ordered
- [ ] 16. WHERE vs HAVING
- [ ] 17. Talking about performance after the query
- [ ] Remember these 10 things

---

## The example tables

All queries below use these tables. The syntax is PostgreSQL. MySQL differences are noted in comments.

```
customers   (id, name, email, country, created_at)
orders      (id, customer_id, status, total, created_at)
order_items (id, order_id, product_id, qty, price)
products    (id, sku, name, category_id, price)
employees   (id, name, department_id, salary, manager_id)
```

**How to talk in a SQL round**
Say your plan out loud before you type: "First I get the rows I need, then I group, then I filter the groups, then I sort." At the end, say which **index** would make it fast. That is the senior touch.

---

## 1. In what order does SQL really run a query?

**In simple words**
You write `SELECT` first, but the database runs it almost last. Knowing the real order explains many errors.

```
FROM / JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

**🗣️ Say this in the interview**
> "SQL runs `FROM` and `JOIN` first, then `WHERE`, `GROUP BY`, `HAVING`, and only then `SELECT`, then `ORDER BY` and `LIMIT`.
> That's why I can't use a `SELECT` alias in `WHERE`: it doesn't exist yet. But I usually can use it in `ORDER BY`.
> It's also why `WHERE` filters **rows** before grouping, and `HAVING` filters **groups** after."

**⚠️ Avoid saying**
"SQL runs top to bottom as written." It doesn't. `SELECT` runs after `WHERE` and `GROUP BY`.

---

## 2. What JOIN types are there?

**In simple words**
A join puts rows from two tables side by side, based on a matching column.

| Join | Returns |
|---|---|
| `INNER JOIN` | Only rows that match on **both** sides |
| `LEFT JOIN` | **All** rows from the left table, plus matches from the right (NULLs if no match) |
| `RIGHT JOIN` | The mirror of LEFT (rarely used, just swap the tables) |
| `FULL OUTER JOIN` | Everything from both sides (not in MySQL, emulate it with UNION) |
| `CROSS JOIN` | Every combination (careful: rows × rows) |
| Self join | A table joined to itself (like employees and their managers) |

**🗣️ Say this in the interview**
> "`INNER JOIN` keeps only matches on both sides. `LEFT JOIN` keeps every row from the left table and fills NULLs when there's no match. I almost never use `RIGHT JOIN`; I just swap the tables. `CROSS JOIN` gives every combination, so I'm careful with it. A self join is a table joined to itself, like employees and managers."

**⚠️ Avoid saying**
"MySQL supports FULL OUTER JOIN." It doesn't. You combine a LEFT and a RIGHT join with UNION.

---

## 3. Find the second-highest salary

**Task:** Return the second-highest salary from `employees`.

```sql
-- 1) Simple
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

-- 2) Window function (handles ties properly, easy to change to Nth)
SELECT DISTINCT salary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employees
) ranked
WHERE rnk = 2;

-- 3) Classic subquery
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

**How it works**
- Version 1: remove duplicate salaries, sort high to low, skip the first one, take the next one.
- Version 2: `DENSE_RANK` gives each distinct salary a rank (1, 2, 3…) with no gaps. Then keep rank 2. Change `2` to any N.
- Version 3: find the top salary, then find the biggest salary that is smaller than it.

**🗣️ Say this in the interview**
> "I'd use `DENSE_RANK`. If two people share the top salary, the 'second highest' should still be the next distinct value. And it's easy to change to the Nth highest."

---

## 4. ROW_NUMBER vs RANK vs DENSE_RANK?

**In simple words**
All three number the rows. They only differ when there is a tie.

Salaries: 100, 90, 90, 80

| salary | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 100 | 1 | 1 | 1 |
| 90 | 2 | 2 | 2 |
| 90 | 3 | 2 | 2 |
| 80 | 4 | **4** | **3** |

**Picture it**
`RANK` is like Olympic medals: two golds, then the next person is fourth. `DENSE_RANK` has no gaps: two golds, then silver.

**🗣️ Say this in the interview**
> "`ROW_NUMBER` is always unique, so I use it to 'pick exactly one per group' or to remove duplicates. `RANK` leaves gaps after ties, like Olympic medals. `DENSE_RANK` has no gaps, so I use it for 'Nth highest value'."

---

## 5. Highest-paid employee in each department

**Task:** For each department, return the employee with the highest salary.

```sql
SELECT department_id, name, salary
FROM (
  SELECT e.*,
         ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
  FROM employees e
) t
WHERE rn = 1;
```

**How it works**
- `PARTITION BY department_id` splits the rows into one group per department.
- Inside each group, rows are sorted by salary, highest first, and numbered 1, 2, 3…
- The outer query keeps only row number 1 in each department.

**🗣️ Say this in the interview**
> "`PARTITION BY` restarts the numbering for each department. If I wanted **everyone** tied for the top, I'd use `RANK` instead of `ROW_NUMBER`."

---

## 6. Find duplicate emails, then delete them (keep the oldest)

**Task:** Show which emails appear more than once, then delete the extra rows and keep the one with the lowest `id`.

```sql
-- Find
SELECT email, COUNT(*) AS cnt
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- Delete all but the lowest id per email
DELETE FROM customers
WHERE id IN (
  SELECT id FROM (
    SELECT id, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
    FROM customers
  ) t
  WHERE rn > 1
);
```

**How it works**
- Find: group by email and keep groups with more than one row.
- Delete: number the rows inside each email group by `id`. The oldest (lowest id) gets 1.
- Every row with number 2 or more is a duplicate, so its `id` goes into the delete list.

**🗣️ Say this in the interview**
> "The extra derived table (the inner `SELECT ... ) t`) also makes this work in MySQL. MySQL doesn't let you select directly from the table you're deleting from. After the cleanup, I'd add a **unique index** on `email` so it can't happen again."

---

## 7. Customers who never placed an order

**Task:** Return all customers with zero orders.

```sql
-- LEFT JOIN + IS NULL
SELECT c.*
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
WHERE o.id IS NULL;

-- NOT EXISTS (my preference: clear, and NULL-safe)
SELECT c.*
FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);
```

**How it works**
- LEFT JOIN version: every customer is kept. Customers with no order get NULL in the order columns. Keep only those.
- NOT EXISTS version: for each customer, check "is there any order for this customer?" Keep the ones where the answer is no.

**🗣️ Say this in the interview**
> "I prefer `NOT EXISTS`. It reads clearly and it's safe with NULLs. A trap worth knowing: `WHERE id NOT IN (SELECT customer_id FROM orders)` returns **no rows at all** if any `customer_id` is NULL. `NOT EXISTS` doesn't have that problem."

---

## 8. Total spent per customer (top 10)

**Task:** Show the 10 customers who spent the most on paid orders, including the number of orders.

```sql
SELECT c.id, c.name, COALESCE(SUM(o.total), 0) AS total_spent, COUNT(o.id) AS orders
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id AND o.status = 'paid'
GROUP BY c.id, c.name
ORDER BY total_spent DESC
LIMIT 10;
```

**How it works**
- LEFT JOIN each customer to their **paid** orders only.
- Group by customer. `SUM` adds the totals. `COALESCE(..., 0)` turns "no orders" (NULL) into 0.
- `COUNT(o.id)` counts only real orders, because NULLs are not counted.
- Sort by total, biggest first, keep 10.

**🗣️ Say this in the interview**
> "The status filter is in the **ON** clause, not in `WHERE`. If I put it in `WHERE`, customers with no paid orders would disappear, because it turns the LEFT JOIN into an INNER JOIN. That's a very common bug."

---

## 9. Running total and 7-day moving average

**Task:** For each day, show paid revenue, the running total so far, and the average of the last 7 days.

```sql
SELECT DATE(created_at) AS day,
       SUM(total) AS revenue,
       SUM(SUM(total)) OVER (ORDER BY DATE(created_at)) AS running_total,
       AVG(SUM(total)) OVER (
         ORDER BY DATE(created_at) ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
       ) AS moving_avg_7d
FROM orders
WHERE status = 'paid'
GROUP BY DATE(created_at)
ORDER BY day;
```

**How it works**
- `GROUP BY DATE(created_at)` makes one row per day. `SUM(total)` is that day's revenue.
- `SUM(SUM(total)) OVER (ORDER BY day)` adds up every day's revenue from the start until this day.
- `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW` means "this row and the 6 before it", so 7 days. `AVG` averages them.

Small example:

| day | revenue | running_total |
|---|---|---|
| Jan 1 | 100 | 100 |
| Jan 2 | 50 | 150 |
| Jan 3 | 70 | 220 |

**🗣️ Say this in the interview**
> "The inner `SUM(total)` is the normal daily total. The window function then runs **over** those daily rows. The moving average assumes every day has orders. If some days are empty, I'd join to a calendar table first."

---

## 10. Month-over-month revenue growth

**Task:** For each month, show revenue, last month's revenue, and the growth in percent.

```sql
WITH monthly AS (
  SELECT DATE_TRUNC('month', created_at) AS month,     -- MySQL: DATE_FORMAT(created_at, '%Y-%m-01')
         SUM(total) AS revenue
  FROM orders
  WHERE status = 'paid'
  GROUP BY 1
)
SELECT month,
       revenue,
       LAG(revenue) OVER (ORDER BY month) AS prev_month,
       ROUND(100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
             / NULLIF(LAG(revenue) OVER (ORDER BY month), 0), 1) AS growth_pct
FROM monthly
ORDER BY month;
```

**How it works**
- The CTE `monthly` (a named temporary result, written with `WITH`) makes one row per month with its revenue.
- `LAG(revenue)` reads the revenue from the previous row, so the previous month.
- Growth = (this month − last month) ÷ last month × 100, rounded to 1 decimal.
- `NULLIF(x, 0)` turns 0 into NULL, so we never divide by zero.

**🗣️ Say this in the interview**
> "`LAG` reads the previous row's value. `NULLIF(…, 0)` avoids dividing by zero. And the CTE keeps it readable: first build monthly totals, then compare them."

---

## 11. Top 3 products per category by revenue

**Task:** For each category, return the 3 products that made the most revenue.

```sql
WITH product_revenue AS (
  SELECT p.category_id, p.id, p.name, SUM(oi.qty * oi.price) AS revenue
  FROM order_items oi
  JOIN products p ON p.id = oi.product_id
  GROUP BY p.category_id, p.id, p.name
)
SELECT category_id, name, revenue
FROM (
  SELECT pr.*,
         DENSE_RANK() OVER (PARTITION BY category_id ORDER BY revenue DESC) AS rnk
  FROM product_revenue pr
) ranked
WHERE rnk <= 3
ORDER BY category_id, revenue DESC;
```

**How it works**
- Step 1 (the CTE): revenue per product = sum of `qty × price` from its order items.
- Step 2: rank products inside each category, highest revenue first.
- Step 3: keep ranks 1 to 3, then sort for a clean result.

**🗣️ Say this in the interview**
> "This is the 'top N per group' pattern: calculate, then rank with `PARTITION BY`, then filter on the rank. I used `DENSE_RANK`, so if two products tie, both are shown. If I needed exactly 3 rows per category, I'd use `ROW_NUMBER`."

---

## 12. Employees who earn more than their manager (self join)

**Task:** Return employees whose salary is higher than their manager's salary.

```sql
SELECT e.name AS employee, e.salary, m.name AS manager, m.salary AS manager_salary
FROM employees e
JOIN employees m ON m.id = e.manager_id
WHERE e.salary > m.salary;
```

**How it works**
- Use the `employees` table twice: `e` is the employee, `m` is the manager.
- Join each employee to the row whose `id` equals their `manager_id`.
- Keep only rows where the employee's salary is bigger.

**🗣️ Say this in the interview**
> "This is a self join: the same table with two aliases. Because it's an INNER JOIN, people with no manager, like the CEO, are left out, which is what we want here."

---

## 13. Orders per day by status, last 30 days (conditional aggregation)

**Task:** For each of the last 30 days, show total orders, paid orders, cancelled orders, and paid revenue as columns.

```sql
SELECT DATE(created_at) AS day,
       COUNT(*) AS total_orders,
       SUM(CASE WHEN status = 'paid'      THEN 1 ELSE 0 END) AS paid,
       SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS cancelled,
       SUM(CASE WHEN status = 'paid' THEN total ELSE 0 END)  AS paid_revenue
FROM orders
WHERE created_at >= CURRENT_DATE - INTERVAL '30 days'   -- MySQL: CURRENT_DATE - INTERVAL 30 DAY
GROUP BY DATE(created_at)
ORDER BY day;
```

**How it works**
- `WHERE` keeps only orders from the last 30 days.
- Group by day.
- Each `CASE` gives 1 (or the total) when the row matches, else 0. `SUM` adds them up, so each status becomes its own column.

**🗣️ Say this in the interview**
> "`CASE` inside `SUM` turns rows into columns, a **pivot**, and it works in every database. PostgreSQL also has `COUNT(*) FILTER (WHERE …)`, which is cleaner."

---

## 14. Repeat customer rate

**Task:** What percentage of paying customers placed more than one paid order?

```sql
SELECT ROUND(100.0 * SUM(CASE WHEN order_count > 1 THEN 1 ELSE 0 END) / COUNT(*), 1)
       AS repeat_customer_pct
FROM (
  SELECT customer_id, COUNT(*) AS order_count
  FROM orders
  WHERE status = 'paid'
  GROUP BY customer_id
) per_customer;
```

**How it works**
- Inner query: one row per customer with their number of paid orders.
- Outer query: count customers with more than 1 order, divide by all customers in the list.
- `100.0` (with a decimal) forces decimal division, so we don't get 0 from integer division.

**🗣️ Say this in the interview**
> "I first build one row per customer, then do the math on top. Note this is the rate among customers who paid at least once. If they want it out of all registered customers, I'd start from the `customers` table with a LEFT JOIN instead."

---

## 15. Products never ordered

**Task:** Return all products that appear in no order item.

```sql
SELECT p.id, p.sku, p.name
FROM products p
WHERE NOT EXISTS (SELECT 1 FROM order_items oi WHERE oi.product_id = p.id);
```

**How it works**
- For each product, check whether any `order_items` row points to it.
- Keep products where no such row exists.

**🗣️ Say this in the interview**
> "Same pattern as 'customers who never ordered'. I use `NOT EXISTS` because it's NULL-safe, and an index on `order_items (product_id)` makes the check fast."

---

## 16. WHERE vs HAVING in one example

**Task:** Find customers with 5 or more orders since 1 January 2026.

```sql
SELECT customer_id, COUNT(*) AS orders
FROM orders
WHERE created_at >= '2026-01-01'   -- filters ROWS before grouping (can use an index)
GROUP BY customer_id
HAVING COUNT(*) >= 5;              -- filters GROUPS after aggregation
```

**How it works**
- `WHERE` throws away old orders first.
- `GROUP BY` makes one row per customer with a count.
- `HAVING` keeps only customers whose count is 5 or more.

**🗣️ Say this in the interview**
> "I put as much filtering as possible in `WHERE`. It cuts rows **before** the expensive grouping, and it can use indexes. `HAVING` is only for conditions on the grouped result, like `COUNT(*)`."

**⚠️ Avoid saying**
"WHERE and HAVING are the same, you can use either." `WHERE` can't see aggregates, and `HAVING` runs after the grouping work is already done.

---

## 17. After writing the query: talk about performance

**In simple words**
When the query is done, don't stop. Say which indexes help and how you'd check it. This is what makes your answer sound senior.

**🗣️ Say this in the interview**
> "For these queries I'd want indexes like:
> - `orders (customer_id)` for the joins,
> - `orders (status, created_at)` for 'paid orders in a date range',
> - `order_items (product_id)` and `order_items (order_id)`,
> - and `customers (email)` as a **unique** index.
>
> Then I'd check with `EXPLAIN ANALYZE` on realistic data. For heavy reports like monthly revenue, I'd run them on a **read replica**, or pre-calculate them into a summary table, so they don't compete with checkout traffic."

For more on indexes and `EXPLAIN`, see `3-backend/05-databases.md`.

---

## Remember these 10 things
1. Real order: FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.
2. Say your plan out loud first: get rows, group, filter groups, sort.
3. `ROW_NUMBER` = exactly one per group. `RANK` = gaps after ties. `DENSE_RANK` = no gaps, use it for Nth highest.
4. Top N per group = window function with `PARTITION BY`, then filter on the rank in an outer query.
5. With a LEFT JOIN, put filters on the right table in `ON`, not `WHERE`, or it becomes an INNER JOIN.
6. Use `NOT EXISTS`, not `NOT IN`. `NOT IN` returns nothing if the subquery has a NULL.
7. Remove duplicates with `ROW_NUMBER() ... WHERE rn > 1`, then add a unique index.
8. `LAG` reads the previous row. `NULLIF(x, 0)` avoids divide by zero.
9. `SUM(CASE WHEN ... THEN 1 ELSE 0 END)` turns rows into columns (pivot).
10. Always finish with the indexes you'd add, `EXPLAIN ANALYZE`, and "heavy reports go to a read replica".
