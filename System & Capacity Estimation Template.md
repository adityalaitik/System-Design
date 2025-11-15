# System Load Analysis Template

## 1. Scenario Overview

* **Name:**
* **Description:**
* **DAU:**
* **Read/Write Pattern:**
* **Retention:**
* **Data per Write:**

---

## 2. Input Parameters

| Parameter               | Value |
| ----------------------- | ----- |
| DAU                     |       |
| Reads per User per Day  |       |
| Writes per User per Day |       |
| Read:Write Ratio        |       |
| Data per Write (bytes)  |       |
| Retention (days)        |       |

---

## 3. Calculations

### 3.1 Daily Activity

* Reads/day =
* Writes/day =
* Total Requests/day =

### 3.2 Requests per Second (RPS)

* Reads/sec =
* Writes/sec =
* Total RPS =

### 3.3 Storage Growth

* Bytes/day =
* GB/day =
* 10-year storage =

---

## 4. Capacity Planning

* Web/App Instances Needed =
* DB Write Shards Needed =
* DB Read Replicas Needed =

---

## 5. Notes / Insights

*
*

---

# Examples

## Example 1 — Read Heavy (100:1)

### Detailed Explanation with Formulas, Diagrams & Tables

### 1. Overview

A **read-heavy** workload means the system handles significantly more read operations than writes. This pattern is common in social media feeds, content platforms, news aggregators, and dashboards.

### 2. Input Parameters

| Parameter               | Value                |
| ----------------------- | -------------------- |
| DAU                     | 100,000,000          |
| Reads per User per Day  | 100                  |
| Writes per User per Day | 1                    |
| Read:Write Ratio        | 100:1                |
| Data per Write          | 1024 bytes           |
| Retention               | 10 years (3650 days) |

---

### 3. Formulas Used

#### **3.1 Daily Activity**

```
Reads/day  = DAU × Reads per User per Day
Writes/day = DAU × Writes per User per Day
Total Requests/day = Reads/day + Writes/day
```

#### **3.2 Convert to Requests per Second (RPS)**

```
Reads/sec  = Reads/day ÷ 86,400
Writes/sec = Writes/day ÷ 86,400
Total RPS  = Reads/sec + Writes/sec
```

#### **3.3 Storage Calculations**

```
Bytes/day = Writes/day × Data per Write
GB/day = Bytes/day ÷ (1024³)
10-year Storage = GB/day × 3650
```

---

### 4. Step-by-Step Calculations

#### **4.1 Daily Activity**

* Reads/day  = 100,000,000 × 100 = **10,000,000,000**
* Writes/day = 100,000,000 × 1 = **100,000,000**
* Total Requests/day = **10,100,000,000**

#### **4.2 RPS (Requests/Second)**

* Reads/sec = 10,000,000,000 ÷ 86,400 = **115,740.74**
* Writes/sec = 100,000,000 ÷ 86,400 = **1,157.41**
* Total RPS = **116,898.15**

#### **4.3 Storage**

* Bytes/day = 100,000,000 × 1024 = **102,400,000,000 bytes**
* GB/day = 102,400,000,000 ÷ 10⁹ = **102.4 GB/day**
* 10-year storage = 102.4 × 3650 = **373.76 TB**

---

### 5. Capacity Planning

#### **5.1 Web/App Tier**

Assume one instance handles **2000 RPS**:

```
Instances_needed = Total_RPS ÷ 2000
                 = 116,898 ÷ 2000
                 = 58.449 → 59 instances
```

#### **5.2 Database Read Load**

With **90% cache hit rate**, only 10% of reads reach DB:

```
DB_reads/sec = 115,740.74 × 0.1 = 11,574.07
```

If each DB replica handles **5000 reads/sec**:

```
Replicas_needed = 11,574 ÷ 5000 = 2.31 → 3 replicas
```

#### **5.3 DB Write Sharding**

Writes/sec = **1,157.41**
If each shard handles **1,000 writes/sec**:

```
Write_shards = 1157 ÷ 1000 = 1.15 → 2 shards
```

---

### 6. Diagram (ASCII)

```
           ┌─────────────────┐
           │   Users (100M)  │
           └────────┬────────┘
                    │ Requests
                    ▼
           ┌────────────────────┐
           │   Load Balancer    │
           └────────┬───────────┘
                    │
          ┌─────────┴──────────────┐
          │ Web/App Server Pool    │ (59 instances)
          └─────────┬──────────────┘
                    │
        ┌───────────▼─────────────┐
        │        Cache Layer       │ (Redis/Memcached)
        └───────────┬─────────────┘
                    │ (10% misses)
                    ▼
        ┌──────────────────────────┐
        │     DB Read Replicas     │ (3 replicas)
        └──────────────────────────┘
                    │ Writes
                    ▼
        ┌──────────────────────────┐
        │    DB Write Shards       │ (2 shards)
        └──────────────────────────┘
```

---

### 7. Summary Table

| Metric           | Value      |
| ---------------- | ---------- |
| Reads/sec        | 115,740.74 |
| Writes/sec       | 1,157.41   |
| Total RPS        | 116,898.15 |
| Storage/day      | 102.4 GB   |
| 10-Year Storage  | 373.76 TB  |
| Web Instances    | 59         |
| DB Write Shards  | 2          |
| DB Read Replicas | 3          |

---

## Example 2 — Write Heavy (1:5)

### Input

* DAU: 100,000,000
* Reads/user/day: 2
* Writes/user/day: 10
* Read:Write ratio: 1:5
* Data per write: 1024 bytes
* Retention: 10 years

### Results

* Reads/day: 200,000,000
* Writes/day: 1,000,000,000
* Reads/sec: 2,314.81
* Writes/sec: 11,574.07
* Total RPS: 13,888.89
* Storage/day: 1.024 TB
* 10-year storage: 3,737.6 TB
* Web instances: 7
* Write shards: 12

---

## Example 3 — Equal Read/Write (1:1)

### Input

* DAU: 100,000,000
* Reads/user/day: 10
* Writes/user/day: 10
* Read:Write ratio: 1:1
* Data per write: 1024 bytes
* Retention: 10 years

### Results

* Reads/day: 1,000,000,000
* Writes/day: 1,000,000,000
* Reads/sec: 11,574.07
* Writes/sec: 11,574.07
* Total RPS: 23,148.15
* Storage/day: 1.024 TB
* 10-year storage: 3,737.6 TB
* Web instances: 12
* Write shards: 12
