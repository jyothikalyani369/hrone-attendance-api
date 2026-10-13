# Design Decisions

### 1. Indexes
* **Created:** Enforced a unique compound index for `(emp_code, date)` inside the `attendance_logs` collection to manage sequential rules, and a single unique index for `employees(emp_code)`.
* **Rejected:** Dropped status indexes due to low entry cardinality providing zero index scanning improvements.

### 2. Punch-in Race
Concurrent punch requests are validated by the database unique compound index on `(emp_code, date)`. The speediest request commits cleanly, while the trailing thread fails immediately with a `DuplicateKeyError`, caught cleanly by FastAPI to emit an operational `409 Conflict` error code.

### 3. Ties
Leaderboard matching ties use a deterministic secondary sorting key. The database pipeline aggregates by `late_minutes` descending and defaults to `emp_code` ascending to keep sorting deterministic.

### 4. Headcount
To preserve headcount metrics for employees containing zero activity records, the aggregation pipeline queries against the `employees` collection first, using an asynchronous `$lookup` stage to join logs without dropping empty results.

### 5. Scale (100x Data)
To cleanly process a 100x data scaling spike, I would shard the collections using `emp_code` as the shard key to distribute horizontal index operations across active database cluster nodes.
