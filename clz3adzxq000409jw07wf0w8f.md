---
title: "1661. Average Time of Process per Machine Problem"
seoTitle: "1661.Average Time of Process per Machine Problem"
seoDescription: "1661.Average Time of Process per Machine Problem"
datePublished: Fri Dec 03 2021 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clz3adzxq000409jw07wf0w8f
slug: leetcode-average-time-of-process-per-machine-problem
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1722033588486/edea860d-e64f-40a7-85b2-cbaef391804c.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1722033623915/e7c97df2-7706-44ad-8bc6-7f44b5f1ae64.jpeg
tags: mysql, leetcode, problem-solving-skills

---

**Problem Statement:**

Given a table `Activity` containing the columns `machine_id`, `process_id`, and `timestamp`, calculate the average processing time for each machine. The `Activity` table logs the start and end times of each process on various machines. The goal is to find the average processing time per machine, rounded to three decimal places.

**Examples:**

```sql
Activity
+------------+------------+-----------+
| machine_id | process_id | timestamp |
+------------+------------+-----------+
| 1          | 1          | 1         |
| 1          | 1          | 3         |
| 1          | 2          | 2         |
| 1          | 2          | 5         |
| 2          | 1          | 1         |
| 2          | 1          | 2         |
+------------+------------+-----------+

Output:
+------------+----------------+
| machine_id | processing_time|
+------------+----------------+
| 1          | 2.500          |
| 2          | 1.000          |
+------------+----------------+
```

**Approach:**

To calculate the average processing time per machine, we:

1. Join the `Activity` table with itself to pair each start and end timestamp for the same process on the same machine.
    
2. Calculate the duration for each process by subtracting the start timestamp from the end timestamp.
    
3. Aggregate these durations by `machine_id` to compute the average processing time, rounding the result to three decimal places.
    

**Solution:**

```sql
SELECT 
    a1.machine_id, 
    ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time 
FROM 
    Activity a1
JOIN 
    Activity a2 
ON 
    a1.machine_id = a2.machine_id  
    AND a1.process_id = a2.process_id 
    AND a2.timestamp > a1.timestamp
GROUP BY 
    a1.machine_id;
```

**Explanation:**

* **Join Condition:** The table is joined with itself based on `machine_id` and `process_id` with the condition that the end timestamp (`a2.timestamp`) is greater than the start timestamp (`a1.timestamp`).
    
* **Duration Calculation:** For each pair of rows from the join, the duration is calculated as the difference between `a2.timestamp` and `a1.timestamp`.
    
* **Average Calculation:** The average of these durations is computed for each `machine_id` and rounded to three decimal places.
    

**Analysis:**

* **Time Complexity:** O(n^2) due to the self-join operation, where n is the number of rows in the table.
    
* **Space Complexity:** O(n) as the join operation and subsequent calculations use additional space proportional to the number of rows.
    

**Optimization:**

This solution is optimal for the given problem constraints. The self-join ensures accurate pairing of start and end timestamps, and the use of aggregate functions efficiently computes the desired result.

**Conclusion:**

This SQL query calculates the average processing time per machine by leveraging self-joins and aggregate functions. The solution is both efficient and easy to understand, providing accurate results for the given problem.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**