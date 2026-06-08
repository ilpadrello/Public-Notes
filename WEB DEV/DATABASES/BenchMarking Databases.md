## 4. How to Benchmark (Step-by-Step)

### Step A: Generate Synthetic Data

Don't write 1 million lines by hand. Use a script to generate "Fake" but "Realistic" data.

SQL

```
-- Postgres trick to generate 1 million rows of random emails
INSERT INTO users (email, active)
SELECT 
    md5(random()::text) || '@gmail.com', 
    (random() > 0.5)::int 
FROM generate_series(1, 1000000);
```

### Step B: Clear the Buffers (Crucial!)

Between testing Solution A and Solution B, you **must** clear the cache. Otherwise, Solution B will look 100x faster just because the data is already sitting in RAM from the first test.

- Restart the database service.
    
- Drop the OS filesystem cache (on Linux: `echo 3 > /proc/sys/vm/drop_caches`).
    

### Step C: Use a Benchmarking Tool

Don't just run one query once. Run it 1,000 times and take the **Average** and the **95th Percentile (p95)**.

- **pgbench:** This is the built-in tool for Postgres. It can simulate 50 users all hitting the database at the same time.
    

---

## 5. What to look for?

When comparing your solutions, look at these three metrics:

1. **Latency:** How long does one query take? (e.g., 5ms vs 500ms).
    
2. **Throughput:** How many queries can the DB handle per second?
    
3. **Index Size:** Use `\di+` in Postgres to see how big your B+ Tree is. If Solution C is 20% faster but the index is 500% larger, it might not be worth it!
    

---

## The "Golden Rule" of Benchmarking

**Only change one thing at a time.** If you change the Index _and_ the Data Type _and_ the Query at the same time, you won't know which one actually fixed the problem!

**Would you like me to show you a specific `pgbench` command to get you started, or do you want to try writing a data-generation script first?**

## 1. The Infrastructure: VM vs. Cloud

If you use AWS RDS or a managed database, you are right—you don't control the "metal." This makes benchmarking hard because AWS might move your DB to a different physical host mid-test.

**The Best Way:** Use a **Local Virtual Machine (VM)** or a **Docker Container** with resource limits.

- **The "2GB RAM" Idea:** Yes! This is a great move. By limiting the RAM, you force the database to hit the "Disk" (your SSD) much sooner.
    
- **How to do it (Docker example):** `docker run --name pg-test -m 2g --cpus=2 -e POSTGRES_PASSWORD=pass -d postgres` This creates a "box" that literally cannot use more than 2GB of RAM, no matter how much your laptop has.
    

## 2. The Data: Fake vs. Production

This depends on **Privacy** and **Complexity**.

- **The Production Copy (Best for Accuracy):** If you can safely "Anonymize" your data (remove real names/emails), using a copy of production is best because it has the "real-world messiness" (uneven distribution of data).
    
- **The Scripted Function (Best for Scale):** If you want to test "What happens at 1 Billion rows," you obviously can't copy that from a smaller production DB. You should use a SQL function.
    

**Literal SQL to generate 10 million rows in seconds:**

SQL

```
INSERT INTO users (email, active, created_at)
SELECT 
    'user_' || i || '@' || (CASE WHEN i % 3 = 0 THEN 'gmail.com' ELSE 'outlook.com' END),
    (CASE WHEN i % 10 = 0 THEN 1 ELSE 0 END), -- 10% active
    now() - (random() * interval '365 days')
FROM generate_series(1, 10000000) s(i);
```

---

## 3. The Execution: How to literally run the test

Don't just copy-paste a query into a terminal and look at your watch. Humans are slow; we need a tool that records **milliseconds**.

### Step 1: Use `pgbench` (The Industry Standard)

Postgres comes with a tool called `pgbench`. It is a command-line tool. You don't "install a service"; you run it from your terminal.

**The "Read-Only" Test Command:**

Bash

```
pgbench -c 10 -j 2 -t 1000 -S -D var1=target_email my_database
```

- `-c 10`: Simulate 10 users hitting the DB at the same time.
    
- `-t 1000`: Each user runs 1,000 queries.
    
- `-S`: Simple SELECT-only mode.
    
- **The Result:** It will give you "TPS" (Transactions Per Second). If Solution A gives 5,000 TPS and Solution B gives 12,000 TPS, you have a clear winner.
    

### Step 2: The "Explain" check

Before running the big benchmark, manually run your query with `EXPLAIN (ANALYZE, BUFFERS)`.

SQL

```
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM users WHERE email LIKE '%@gmail.com%' AND active = 1;
```

Look for **"Shared Read"**.

- If "Shared Read" is high, the DB went to the disk.
    
- If "Shared Hit" is high, the DB cheated and used RAM. **Always check this** or your benchmark results will be lies!
    

---

## 4. The Scientific Protocol

To be a "Pro," follow this sequence every single time you test a new idea:

1. **Cold Start:** Restart your Docker/VM (clears the DB RAM).
    
2. **Warm-up:** Run the query 5 times (this fills the "Buffer Pool" with the index).
    
3. **The Benchmark:** Run `pgbench` for 60 seconds.
    
4. **The Record:** Note the **Latency (Average)** and **Latency (95th Percentile)**.
    
    - _The 95th percentile is the most important—it tells you how slow the "slowest" users felt._
        

---

## Summary of your Lab Setup:

1. **Environment:** Docker container with a 2GB RAM limit.
    
2. **Data:** 10M rows generated via `generate_series`.
    
3. **Tool:** `pgbench` for the "Speed Test" and `EXPLAIN ANALYZE` for the "Logic Check."
    

**Would you like me to give you a "Test Script" template where you can just plug in your Table name and Index ideas to see which one wins?**