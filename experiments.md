# MongoDB Laboratory Manual

---

> **Prerequisites:** MongoDB and MongoDB Shell (`mongosh`) are already installed on your system, and you have completed basic CRUD operations. This manual picks up from there and takes you through 9 structured experiments.

---

## How to Start MongoDB Shell

Open your terminal (Command Prompt / PowerShell on Windows, Terminal on Linux/Mac) and type:

```bash
mongosh
```

You should see the `test>` prompt. All commands in this manual are typed at this prompt unless stated otherwise.

---

## Sample Collections Used in This Manual

Before starting the experiments, set up the shared sample data. Run the following in `mongosh` to create and populate the collections inside a database called `labdb`.

```js
use labdb
```

### students collection
```js
db.students.insertMany([
  { name: "Alice",   age: 20, grade: "A", city: "Mumbai",  marks: 92, subject: "DBMS",    fees: 45000 },
  { name: "Bob",     age: 22, grade: "B", city: "Delhi",   marks: 78, subject: "OS",       fees: 38000 },
  { name: "Charlie", age: 21, grade: "A", city: "Mumbai",  marks: 88, subject: "Networks", fees: 42000 },
  { name: "Diana",   age: 23, grade: "C", city: "Pune",    marks: 65, subject: "DBMS",     fees: 30000 },
  { name: "Eve",     age: 20, grade: "B", city: "Chennai", marks: 74, subject: "OS",       fees: 36000 },
  { name: "Frank",   age: 22, grade: "A", city: "Delhi",   marks: 95, subject: "DBMS",     fees: 48000 },
  { name: "Grace",   age: 21, grade: "C", city: "Pune",    marks: 60, subject: "Networks", fees: 28000 },
  { name: "Henry",   age: 24, grade: "B", city: "Mumbai",  marks: 81, subject: "OS",       fees: 40000 }
])
```

**Expected Output:**
```
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('...'), '1': ObjectId('...'), '2': ObjectId('...'),
    '3': ObjectId('...'), '4': ObjectId('...'), '5': ObjectId('...'),
    '6': ObjectId('...'), '7': ObjectId('...')
  }
}
```

### products collection
```js
db.products.insertMany([
  { name: "Laptop",     category: "Electronics", price: 55000, stock: 10, tags: ["portable", "computing"] },
  { name: "Phone",      category: "Electronics", price: 18000, stock: 50, tags: ["mobile", "computing"]   },
  { name: "Desk",       category: "Furniture",   price: 8500,  stock: 5,  tags: ["office", "wooden"]      },
  { name: "Chair",      category: "Furniture",   price: 3200,  stock: 20, tags: ["office", "ergonomic"]   },
  { name: "Headphones", category: "Electronics", price: 3500,  stock: 35, tags: ["audio", "portable"]     },
  { name: "Tablet",     category: "Electronics", price: 25000, stock: 15, tags: ["portable", "computing"] },
  { name: "Monitor",    category: "Electronics", price: 14000, stock: 8,  tags: ["display", "computing"]  },
  { name: "Bookshelf",  category: "Furniture",   price: 6000,  stock: 12, tags: ["wooden", "storage"]     }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '7': ObjectId('...') } }
```

### employees collection
```js
db.employees.insertMany([
  { name: "Raj",    dept: "HR",        salary: 45000, skills: ["communication", "excel"],          location: { city: "Mumbai",    lat: 19.076, lon: 72.877 } },
  { name: "Priya",  dept: "IT",        salary: 72000, skills: ["python", "mongodb", "docker"],      location: { city: "Bangalore", lat: 12.971, lon: 77.594 } },
  { name: "Anil",   dept: "Finance",   salary: 60000, skills: ["excel", "tally", "accounting"],     location: { city: "Delhi",     lat: 28.704, lon: 77.102 } },
  { name: "Sneha",  dept: "IT",        salary: 85000, skills: ["java", "mongodb", "kubernetes"],    location: { city: "Pune",      lat: 18.520, lon: 73.856 } },
  { name: "Karan",  dept: "HR",        salary: 42000, skills: ["communication", "recruitment"],     location: { city: "Chennai",   lat: 13.083, lon: 80.270 } },
  { name: "Meena",  dept: "IT",        salary: 95000, skills: ["python", "ml", "tensorflow"],       location: { city: "Bangalore", lat: 12.971, lon: 77.594 } },
  { name: "Suresh", dept: "Finance",   salary: 55000, skills: ["excel", "sap", "accounting"],       location: { city: "Hyderabad", lat: 17.385, lon: 78.486 } },
  { name: "Divya",  dept: "Marketing", salary: 48000, skills: ["social media", "excel", "content"], location: { city: "Mumbai",    lat: 19.076, lon: 72.877 } }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '7': ObjectId('...') } }
```

### catalog collection (used in Experiment 9 for Text Search)
```js
db.catalog.insertMany([
  { title: "Introduction to MongoDB",  description: "A beginner guide to MongoDB database and NoSQL concepts." },
  { title: "Advanced Aggregation",     description: "Learn how to use aggregation pipelines for complex data analysis." },
  { title: "Indexing in MongoDB",      description: "Explore how indexes improve query performance in MongoDB." },
  { title: "Python with MongoDB",      description: "Connect Python applications to MongoDB using PyMongo driver." },
  { title: "Data Modeling NoSQL",      description: "Design schemas and data models for NoSQL databases." },
  { title: "Replication and Sharding", description: "Understand MongoDB replication sets and horizontal sharding." },
  { title: "MongoDB Security Guide",   description: "Authentication, authorization, and encryption in MongoDB." },
  { title: "Full Text Search MongoDB", description: "Implement full text search and relevance scoring using MongoDB Atlas." }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '7': ObjectId('...') } }
```

> **Quick reference — sample data summary**
>
> | Name    | Grade | City    | Marks | Subject  | Fees  |
> |---------|-------|---------|-------|----------|-------|
> | Alice   | A     | Mumbai  | 92    | DBMS     | 45000 |
> | Bob     | B     | Delhi   | 78    | OS       | 38000 |
> | Charlie | A     | Mumbai  | 88    | Networks | 42000 |
> | Diana   | C     | Pune    | 65    | DBMS     | 30000 |
> | Eve     | B     | Chennai | 74    | OS       | 36000 |
> | Frank   | A     | Delhi   | 95    | DBMS     | 48000 |
> | Grace   | C     | Pune    | 60    | Networks | 28000 |
> | Henry   | B     | Mumbai  | 81    | OS       | 40000 |

---

---

# EXPERIMENT 1

## Illustration of WHERE Clause, AND/OR Operations & Basic CRUD Commands

**Aim:**
- Part A: Demonstrate WHERE-equivalent filtering using AND and OR logical operators in MongoDB.
- Part B: Perform Insert, Query, Update, Delete, and Projection operations on a collection.

---

### Part A — Filtering with AND / OR (WHERE Clause Equivalent)

In relational databases you use `WHERE col = value`. In MongoDB, you pass a **filter document** to `find()`. Logical operators `$and` and `$or` work just like `AND` / `OR` in SQL.

#### 1. Simple WHERE equivalent (single condition)

```js
// SQL: SELECT * FROM students WHERE city = 'Mumbai'
db.students.find({ city: "Mumbai" }, { name: 1, city: 1, grade: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai', grade: 'A', marks: 92 }
{ name: 'Charlie', city: 'Mumbai', grade: 'A', marks: 88 }
{ name: 'Henry',   city: 'Mumbai', grade: 'B', marks: 81 }
```

---

#### 2. AND operation — both conditions must be true

```js
// SQL: SELECT * FROM students WHERE city = 'Mumbai' AND grade = 'A'
db.students.find({
  $and: [
    { city: "Mumbai" },
    { grade: "A" }
  ]
}, { name: 1, city: 1, grade: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai', grade: 'A' }
{ name: 'Charlie', city: 'Mumbai', grade: 'A' }
```

> **Shorthand** — MongoDB applies AND by default when you list multiple fields in one object:
> ```js
> db.students.find({ city: "Mumbai", grade: "A" }, { name: 1, city: 1, grade: 1, _id: 0 })
> ```
> This produces the exact same output as above.

---

#### 3. OR operation — at least one condition must be true

```js
// SQL: SELECT * FROM students WHERE city = 'Delhi' OR grade = 'A'
db.students.find({
  $or: [
    { city: "Delhi" },
    { grade: "A" }
  ]
}, { name: 1, city: 1, grade: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai', grade: 'A' }
{ name: 'Bob',     city: 'Delhi',  grade: 'B' }
{ name: 'Charlie', city: 'Mumbai', grade: 'A' }
{ name: 'Frank',   city: 'Delhi',  grade: 'A' }
```
> Alice and Charlie appear because they have grade A; Bob and Frank appear because they are from Delhi.

---

#### 4. Combining AND with OR

```js
// Students from Mumbai  OR  (grade A AND marks > 85)
db.students.find({
  $or: [
    { city: "Mumbai" },
    { $and: [{ grade: "A" }, { marks: { $gt: 85 } }] }
  ]
}, { name: 1, city: 1, grade: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai', grade: 'A', marks: 92 }
{ name: 'Charlie', city: 'Mumbai', grade: 'A', marks: 88 }
{ name: 'Frank',   city: 'Delhi',  grade: 'A', marks: 95 }
{ name: 'Henry',   city: 'Mumbai', grade: 'B', marks: 81 }
```
> Frank is included because grade=A AND marks=95>85 (even though he is not from Mumbai).
> Henry is included because he is from Mumbai (even though grade=B).

---

### Part B — Insert, Query, Update, Delete & Projection

#### 1. INSERT — insertOne() and insertMany()

```js
// Insert a single document
db.students.insertOne({
  name: "Ishaan",
  age: 21,
  grade: "B",
  city: "Kolkata",
  marks: 76,
  subject: "DBMS",
  fees: 37000
})
```

**Expected Output:**
```
{ acknowledged: true, insertedId: ObjectId('...') }
```

```js
// Insert multiple documents at once
db.students.insertMany([
  { name: "Jaya",  age: 22, grade: "A", city: "Hyderabad", marks: 90, subject: "OS",   fees: 44000 },
  { name: "Kabir", age: 20, grade: "C", city: "Pune",      marks: 62, subject: "DBMS", fees: 29000 }
])
```

**Expected Output:**
```
{
  acknowledged: true,
  insertedIds: { '0': ObjectId('...'), '1': ObjectId('...') }
}
```

---

#### 2. QUERY — find() and findOne()

```js
// Return ALL documents (all fields)
db.students.find()
```

**Expected Output:** All 11 documents (8 original + Ishaan, Jaya, Kabir) — each printed as a full document with all fields. Output is abbreviated below for readability.
```
{ _id: ObjectId('...'), name: 'Alice',   age: 20, grade: 'A', city: 'Mumbai',    marks: 92, subject: 'DBMS',    fees: 45000 }
{ _id: ObjectId('...'), name: 'Bob',     age: 22, grade: 'B', city: 'Delhi',     marks: 78, subject: 'OS',      fees: 38000 }
... (9 more documents)
```

```js
// Return the FIRST document where grade = 'A'
db.students.findOne({ grade: "A" })
```

**Expected Output:**
```
{
  _id: ObjectId('...'),
  name: 'Alice',
  age: 20,
  grade: 'A',
  city: 'Mumbai',
  marks: 92,
  subject: 'DBMS',
  fees: 45000
}
```

```js
// Count documents where grade = 'A'
db.students.countDocuments({ grade: "A" })
```

**Expected Output:**
```
4
```
> Alice, Charlie, Frank, and Jaya (just inserted) all have grade A.

---

#### 3. UPDATE — updateOne() and updateMany()

```js
// Update Bob's grade from 'B' to 'A'
db.students.updateOne(
  { name: "Bob" },
  { $set: { grade: "A" } }
)
```

**Expected Output:**
```
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedId: null
}
```

```js
// Increase fees by 5000 for ALL students in Pune
db.students.updateMany(
  { city: "Pune" },
  { $inc: { fees: 5000 } }
)
```

**Expected Output:**
```
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 3,
  modifiedCount: 3,
  upsertedId: null
}
```
> Diana, Grace, and Kabir (all in Pune) are updated. Their fees become 35000, 33000, and 34000 respectively.

```js
// Verify the update
db.students.find({ city: "Pune" }, { name: 1, city: 1, fees: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Diana', city: 'Pune', fees: 35000 }
{ name: 'Grace', city: 'Pune', fees: 33000 }
{ name: 'Kabir', city: 'Pune', fees: 34000 }
```

```js
// Replace an entire document (use with caution — replaces ALL fields except _id)
db.students.replaceOne(
  { name: "Kabir" },
  { name: "Kabir", age: 21, grade: "B", city: "Pune", marks: 70, subject: "DBMS", fees: 34000 }
)
```

**Expected Output:**
```
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedId: null
}
```

---

#### 4. DELETE — deleteOne() and deleteMany()

```js
// Delete Kabir's document
db.students.deleteOne({ name: "Kabir" })
```

**Expected Output:**
```
{ acknowledged: true, deletedCount: 1 }
```

```js
// Delete ALL documents where grade = 'C'
db.students.deleteMany({ grade: "C" })
```

**Expected Output:**
```
{ acknowledged: true, deletedCount: 2 }
```
> Diana and Grace (both grade C) are removed.

```js
// Confirm deletion — this should return an empty array
db.students.find({ grade: "C" }, { name: 1, _id: 0 })
```

**Expected Output:**
```
// (no documents returned)
```

---

#### 5. PROJECTION — show only specific fields

```js
// Show only name and city for all students — suppress _id
db.students.find({}, { name: 1, city: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai'    }
{ name: 'Bob',     city: 'Delhi'     }
{ name: 'Charlie', city: 'Mumbai'    }
{ name: 'Eve',     city: 'Chennai'   }
{ name: 'Frank',   city: 'Delhi'     }
{ name: 'Henry',   city: 'Mumbai'    }
{ name: 'Ishaan',  city: 'Kolkata'   }
{ name: 'Jaya',    city: 'Hyderabad' }
```
> Diana and Grace were deleted in the previous step. Kabir was also deleted.

```js
// Show all fields EXCEPT fees
db.students.find({}, { fees: 0, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   age: 20, grade: 'A', city: 'Mumbai',    marks: 92, subject: 'DBMS'    }
{ name: 'Bob',     age: 22, grade: 'A', city: 'Delhi',     marks: 78, subject: 'OS'      }
{ name: 'Charlie', age: 21, grade: 'A', city: 'Mumbai',    marks: 88, subject: 'Networks'}
...
```
> Note: Bob's grade now shows as 'A' because we updated it in step 3.

```js
// Combine filter + projection: grade A students, show only name and marks
db.students.find(
  { grade: "A" },
  { name: 1, marks: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92 }
{ name: 'Bob',     marks: 78 }
{ name: 'Charlie', marks: 88 }
{ name: 'Frank',   marks: 95 }
{ name: 'Jaya',    marks: 90 }
```

> **Key Rule:** In a projection document, you cannot mix `1` (include) and `0` (exclude) except for `_id`. Use either all `1`s (include specific fields) or all `0`s (exclude specific fields).

---

> **Note for remaining experiments:** Re-insert the original 8 students fresh to reset the collection before proceeding.
> ```js
> db.students.drop()
> // Then re-run the insertMany from the setup section above.
> ```

---

**Viva Questions:**
1. What is the difference between `insertOne()` and `insertMany()`?
2. How do you perform an AND operation without using the `$and` operator explicitly?
3. What does `$set` do in an update operation? What happens if the field doesn't exist?
4. What is a projection in MongoDB?

---

---

# EXPERIMENT 2

## Field Selection with Projection & Limiting Results with `limit()` and `find()`

**Aim:**
- Part A: Write a MongoDB query to select certain fields and ignore others from a collection.
- Part B: Display only the first 5 documents from the results obtained in Part A.

> **Assumption:** The original 8 students are present in the collection for this experiment.

---

### Part A — Selecting and Ignoring Fields (Projection)

Projection lets you control exactly **which fields** are returned in the result. This is important for performance — you avoid fetching large fields you don't need.

#### Syntax
```js
db.collection.find( <filter>, <projection> )
```

#### 1. Include specific fields (show only these columns)

```js
// Show only name, subject, and marks for all students
db.students.find(
  {},
  { name: 1, subject: 1, marks: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   subject: 'DBMS',     marks: 92 }
{ name: 'Bob',     subject: 'OS',       marks: 78 }
{ name: 'Charlie', subject: 'Networks', marks: 88 }
{ name: 'Diana',   subject: 'DBMS',     marks: 65 }
{ name: 'Eve',     subject: 'OS',       marks: 74 }
{ name: 'Frank',   subject: 'DBMS',     marks: 95 }
{ name: 'Grace',   subject: 'Networks', marks: 60 }
{ name: 'Henry',   subject: 'OS',       marks: 81 }
```

---

#### 2. Exclude specific fields (hide only these columns)

```js
// Show everything except fees and _id
db.students.find(
  {},
  { fees: 0, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   age: 20, grade: 'A', city: 'Mumbai',  marks: 92, subject: 'DBMS'     }
{ name: 'Bob',     age: 22, grade: 'B', city: 'Delhi',   marks: 78, subject: 'OS'       }
{ name: 'Charlie', age: 21, grade: 'A', city: 'Mumbai',  marks: 88, subject: 'Networks' }
{ name: 'Diana',   age: 23, grade: 'C', city: 'Pune',    marks: 65, subject: 'DBMS'     }
{ name: 'Eve',     age: 20, grade: 'B', city: 'Chennai', marks: 74, subject: 'OS'       }
{ name: 'Frank',   age: 22, grade: 'A', city: 'Delhi',   marks: 95, subject: 'DBMS'     }
{ name: 'Grace',   age: 21, grade: 'C', city: 'Pune',    marks: 60, subject: 'Networks' }
{ name: 'Henry',   age: 24, grade: 'B', city: 'Mumbai',  marks: 81, subject: 'OS'       }
```

---

#### 3. Projection with a filter condition

```js
// For students in Mumbai, show only name and marks
db.students.find(
  { city: "Mumbai" },
  { name: 1, marks: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92 }
{ name: 'Charlie', marks: 88 }
{ name: 'Henry',   marks: 81 }
```

---

#### 4. Nested field projection (on employees collection)

```js
// Show employee name and only the city sub-field from the nested location object
db.employees.find(
  {},
  { name: 1, "location.city": 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Raj',    location: { city: 'Mumbai'    } }
{ name: 'Priya',  location: { city: 'Bangalore' } }
{ name: 'Anil',   location: { city: 'Delhi'     } }
{ name: 'Sneha',  location: { city: 'Pune'      } }
{ name: 'Karan',  location: { city: 'Chennai'   } }
{ name: 'Meena',  location: { city: 'Bangalore' } }
{ name: 'Suresh', location: { city: 'Hyderabad' } }
{ name: 'Divya',  location: { city: 'Mumbai'    } }
```

---

### Part B — Limiting Results with `limit()` and `skip()`

#### 1. Basic limit — get only the first 5 documents

```js
// First 5 students in insertion order
db.students.find(
  {},
  { name: 1, marks: 1, _id: 0 }
).limit(5)
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92 }
{ name: 'Bob',     marks: 78 }
{ name: 'Charlie', marks: 88 }
{ name: 'Diana',   marks: 65 }
{ name: 'Eve',     marks: 74 }
```

---

#### 2. limit() combined with sort() — top 5 scorers

```js
// Sort by marks descending, then take the top 5
db.students.find(
  {},
  { name: 1, marks: 1, _id: 0 }
).sort({ marks: -1 }).limit(5)
```

**Expected Output:**
```
{ name: 'Frank',   marks: 95 }
{ name: 'Alice',   marks: 92 }
{ name: 'Charlie', marks: 88 }
{ name: 'Henry',   marks: 81 }
{ name: 'Bob',     marks: 78 }
```

---

#### 3. skip() — skip the first N documents (pagination)

```js
// Skip the top 3 scorers, show the next 5
db.students.find(
  {},
  { name: 1, marks: 1, _id: 0 }
).sort({ marks: -1 }).skip(3).limit(5)
```

**Expected Output:**
```
{ name: 'Henry', marks: 81 }
{ name: 'Bob',   marks: 78 }
{ name: 'Eve',   marks: 74 }
{ name: 'Diana', marks: 65 }
{ name: 'Grace', marks: 60 }
```
> Frank(95), Alice(92), Charlie(88) were skipped. The next 5 are shown above.

> **Pagination Pattern:** To get "page N" of results with `pageSize` documents per page:
> ```js
> .skip((N - 1) * pageSize).limit(pageSize)
> ```

---

#### 4. Apply limit on a filtered query (Part A + limit combined)

```js
// First 5 students from Mumbai, showing name, subject, marks
// (Mumbai only has 3 students, so all 3 are returned)
db.students.find(
  { city: "Mumbai" },
  { name: 1, subject: 1, marks: 1, _id: 0 }
).limit(5)
```

**Expected Output:**
```
{ name: 'Alice',   subject: 'DBMS',     marks: 92 }
{ name: 'Charlie', subject: 'Networks', marks: 88 }
{ name: 'Henry',   subject: 'OS',       marks: 81 }
```
> Only 3 documents match `city: "Mumbai"`, so `.limit(5)` returns all 3.

---

#### 5. countDocuments() — useful for understanding the dataset

```js
// How many A-grade students are there?
db.students.countDocuments({ grade: "A" })
```

**Expected Output:**
```
3
```
> Alice, Charlie, Frank.

---

**Viva Questions:**
1. What is the difference between `limit(5)` and `skip(5)`?
2. Can you mix inclusion (`1`) and exclusion (`0`) in a projection? What is the only exception?
3. How would you implement pagination using `limit()` and `skip()`?
4. Why is projection important in a production application?

---

---

# EXPERIMENT 3

## Query Selectors — Comparison, Logical, Geospatial & Bitwise

**Aim:**
- Part A: Execute comparison and logical query selectors on any collection.
- Part B: Execute geospatial and bitwise query selectors on any collection.

---

### Part A — Comparison & Logical Selectors

#### Comparison Selectors

| Operator | Meaning               | SQL Equivalent  |
|----------|-----------------------|-----------------|
| `$eq`    | Equal to              | `=`             |
| `$ne`    | Not equal to          | `!=`            |
| `$gt`    | Greater than          | `>`             |
| `$gte`   | Greater than or equal | `>=`            |
| `$lt`    | Less than             | `<`             |
| `$lte`   | Less than or equal    | `<=`            |
| `$in`    | In a list             | `IN (...)`      |
| `$nin`   | Not in a list         | `NOT IN (...)`  |

---

```js
// $eq — student with exactly 88 marks
db.students.find({ marks: { $eq: 88 } }, { name: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Charlie', marks: 88 }
```

---

```js
// $ne — students NOT from Mumbai
db.students.find({ city: { $ne: "Mumbai" } }, { name: 1, city: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Bob',   city: 'Delhi'   }
{ name: 'Diana', city: 'Pune'    }
{ name: 'Eve',   city: 'Chennai' }
{ name: 'Frank', city: 'Delhi'   }
{ name: 'Grace', city: 'Pune'    }
```

---

```js
// $gt — students with marks greater than 80
db.students.find({ marks: { $gt: 80 } }, { name: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92 }
{ name: 'Charlie', marks: 88 }
{ name: 'Frank',   marks: 95 }
{ name: 'Henry',   marks: 81 }
```

---

```js
// $gte and $lte — marks between 70 and 90 (inclusive)
db.students.find(
  { marks: { $gte: 70, $lte: 90 } },
  { name: 1, marks: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Bob',     marks: 78 }
{ name: 'Charlie', marks: 88 }
{ name: 'Eve',     marks: 74 }
{ name: 'Henry',   marks: 81 }
```
> Alice(92) and Frank(95) are excluded (above 90). Diana(65) and Grace(60) are excluded (below 70).

---

```js
// $in — students from Mumbai or Delhi
db.students.find(
  { city: { $in: ["Mumbai", "Delhi"] } },
  { name: 1, city: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai' }
{ name: 'Bob',     city: 'Delhi'  }
{ name: 'Charlie', city: 'Mumbai' }
{ name: 'Frank',   city: 'Delhi'  }
{ name: 'Henry',   city: 'Mumbai' }
```

---

```js
// $nin — students NOT from Pune or Chennai
db.students.find(
  { city: { $nin: ["Pune", "Chennai"] } },
  { name: 1, city: 1, _id: 0 }
)
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai' }
{ name: 'Bob',     city: 'Delhi'  }
{ name: 'Charlie', city: 'Mumbai' }
{ name: 'Frank',   city: 'Delhi'  }
{ name: 'Henry',   city: 'Mumbai' }
```
> Diana(Pune), Eve(Chennai), and Grace(Pune) are excluded.

---

#### Logical Selectors

| Operator | Meaning                            |
|----------|------------------------------------|
| `$and`   | All conditions must be true        |
| `$or`    | At least one condition must be true|
| `$nor`   | None of the conditions are true    |
| `$not`   | Negates the specified expression   |

---

```js
// $and — grade A AND marks > 90
db.students.find({
  $and: [
    { grade: "A" },
    { marks: { $gt: 90 } }
  ]
}, { name: 1, grade: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice', grade: 'A', marks: 92 }
{ name: 'Frank', grade: 'A', marks: 95 }
```
> Charlie(88) is grade A but marks are not > 90, so excluded.

---

```js
// $or — city is Pune OR marks < 70
db.students.find({
  $or: [
    { city: "Pune" },
    { marks: { $lt: 70 } }
  ]
}, { name: 1, city: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Diana', city: 'Pune', marks: 65 }
{ name: 'Grace', city: 'Pune', marks: 60 }
```
> Diana and Grace satisfy both conditions (Pune AND marks < 70). No other student is from Pune or has marks < 70.

---

```js
// $nor — NOT from Mumbai AND NOT grade C
db.students.find({
  $nor: [
    { city: "Mumbai" },
    { grade: "C" }
  ]
}, { name: 1, city: 1, grade: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Bob',   city: 'Delhi',   grade: 'B' }
{ name: 'Eve',   city: 'Chennai', grade: 'B' }
{ name: 'Frank', city: 'Delhi',   grade: 'A' }
```
> Alice(Mumbai), Charlie(Mumbai), Henry(Mumbai) excluded — they ARE from Mumbai.
> Diana(C), Grace(C) excluded — they ARE grade C.
> Bob, Eve, Frank match neither condition.

---

```js
// $not — marks NOT greater than 80
db.students.find({
  marks: { $not: { $gt: 80 } }
}, { name: 1, marks: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Bob',   marks: 78 }
{ name: 'Diana', marks: 65 }
{ name: 'Eve',   marks: 74 }
{ name: 'Grace', marks: 60 }
```
> Alice(92), Charlie(88), Frank(95), Henry(81) are all > 80, so excluded.

---

### Part B — Geospatial & Bitwise Selectors

#### Geospatial Selectors

First, create a `places` collection with GeoJSON Point data and a `2dsphere` index:

```js
db.places.insertMany([
  { name: "Gateway of India", loc: { type: "Point", coordinates: [72.8347, 18.9220] } },
  { name: "Marine Drive",     loc: { type: "Point", coordinates: [72.8232, 18.9432] } },
  { name: "Juhu Beach",       loc: { type: "Point", coordinates: [72.8264, 19.0883] } },
  { name: "Bandra Fort",      loc: { type: "Point", coordinates: [72.8193, 19.0541] } },
  { name: "Elephanta Caves",  loc: { type: "Point", coordinates: [72.9313, 18.9633] } }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '4': ObjectId('...') } }
```

```js
// Create a 2dsphere index on the loc field (REQUIRED before any geospatial query)
db.places.createIndex({ loc: "2dsphere" })
```

**Expected Output:**
```
loc_2dsphere
```

---

**`$near` — find places within 10 km of Gateway of India, sorted by distance (nearest first)**

```js
db.places.find({
  loc: {
    $near: {
      $geometry: { type: "Point", coordinates: [72.8347, 18.9220] },
      $maxDistance: 10000   // metres
    }
  }
}, { name: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Gateway of India' }
{ name: 'Marine Drive'     }
{ name: 'Elephanta Caves'  }
```
> Gateway of India is the reference point (0 m away). Marine Drive is ~2.6 km away. Elephanta Caves is ~9.9 km away. Juhu Beach (~18.5 km) and Bandra Fort (~14.7 km) exceed the 10 km limit.

---

**`$geoWithin` — find places inside a rectangular polygon (18.90°N–19.00°N, 72.80°E–72.95°E)**

```js
db.places.find({
  loc: {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [[
          [72.80, 18.90],
          [72.95, 18.90],
          [72.95, 19.00],
          [72.80, 19.00],
          [72.80, 18.90]   // polygon must close: first = last point
        ]]
      }
    }
  }
}, { name: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Gateway of India' }
{ name: 'Marine Drive'     }
{ name: 'Elephanta Caves'  }
```
> Juhu Beach (lat 19.0883 > 19.00) and Bandra Fort (lat 19.0541 > 19.00) lie outside the northern boundary of the polygon.

> **Coordinate order in MongoDB GeoJSON:** `[longitude, latitude]` — note this is the reverse of the usual map convention (lat, lon).

---

#### Bitwise Selectors

Bitwise selectors operate on **integer fields** using binary bit operations. They are useful for compactly storing multiple boolean flags in a single integer.

```js
// Create a permissions collection
db.permissions.insertMany([
  { username: "admin",     permBits: 15 },   // binary: 1111
  { username: "editor",    permBits: 6  },   // binary: 0110
  { username: "viewer",    permBits: 4  },   // binary: 0100
  { username: "moderator", permBits: 7  }    // binary: 0111
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '3': ObjectId('...') } }
```

**Bit layout for reference:**

| username  | permBits | Binary | Bit3 | Bit2 | Bit1 | Bit0 |
|-----------|----------|--------|------|------|------|------|
| admin     | 15       | 1111   | 1    | 1    | 1    | 1    |
| editor    | 6        | 0110   | 0    | 1    | 1    | 0    |
| viewer    | 4        | 0100   | 0    | 1    | 0    | 0    |
| moderator | 7        | 0111   | 0    | 1    | 1    | 1    |

---

**`$bitsAllSet` — ALL specified bits must be 1**

```js
// Bitmask 3 = 0011 — both bit0 AND bit1 must be set
db.permissions.find(
  { permBits: { $bitsAllSet: 3 } },
  { username: 1, permBits: 1, _id: 0 }
)
```

**Expected Output:**
```
{ username: 'admin',     permBits: 15 }
{ username: 'moderator', permBits: 7  }
```
> editor(6=0110): bit0=0 → fails. viewer(4=0100): bit0=0 and bit1=0 → fails.

---

**`$bitsAnySet` — AT LEAST ONE of the specified bits must be 1**

```js
// Bitmask 3 = 0011 — bit0 OR bit1 must be set
db.permissions.find(
  { permBits: { $bitsAnySet: 3 } },
  { username: 1, permBits: 1, _id: 0 }
)
```

**Expected Output:**
```
{ username: 'admin',     permBits: 15 }
{ username: 'editor',    permBits: 6  }
{ username: 'moderator', permBits: 7  }
```
> editor(6=0110) has bit1=1 → passes. viewer(4=0100) has bit0=0 and bit1=0 → fails.

---

**`$bitsAllClear` — ALL specified bits must be 0**

```js
// Bitmask 8 = 1000 — bit3 must be 0
db.permissions.find(
  { permBits: { $bitsAllClear: 8 } },
  { username: 1, permBits: 1, _id: 0 }
)
```

**Expected Output:**
```
{ username: 'editor',    permBits: 6 }
{ username: 'viewer',    permBits: 4 }
{ username: 'moderator', permBits: 7 }
```
> admin(15=1111): bit3=1 → fails. All others have bit3=0 → pass.

---

**`$bitsAnyClear` — AT LEAST ONE of the specified bits must be 0**

```js
// Bitmask 8 = 1000 — bit3 must be 0 in at least one of the checked bits
db.permissions.find(
  { permBits: { $bitsAnyClear: 8 } },
  { username: 1, permBits: 1, _id: 0 }
)
```

**Expected Output:**
```
{ username: 'editor',    permBits: 6 }
{ username: 'viewer',    permBits: 4 }
{ username: 'moderator', permBits: 7 }
```
> Same result here — admin is the only one with bit3=1, so it fails; all others pass.

---

**Viva Questions:**
1. What is the difference between `$in` and `$or`?
2. What index type is required for geospatial queries in MongoDB?
3. What is the coordinate order in MongoDB GeoJSON?
4. Give a real-world use case where bitwise selectors would be useful.

---

---

# EXPERIMENT 4

## Projection Operators — `$`, `$elemMatch`, `$slice`

**Aim:** Create and demonstrate how projection operators `$`, `$elemMatch`, and `$slice` are used in MongoDB.

---

### Background

These special operators are used **inside the projection document** of `find()`. They control what gets returned from **array fields** within documents.

---

### Setup — collections with arrays

```js
// scores collection — array of exam objects per student
db.scores.insertMany([
  { student: "Alice", exams: [{ type: "math",    score: 88 },
                               { type: "science", score: 92 },
                               { type: "english", score: 75 }] },
  { student: "Bob",   exams: [{ type: "math",    score: 65 },
                               { type: "science", score: 70 },
                               { type: "english", score: 80 }] },
  { student: "Carol", exams: [{ type: "math",    score: 95 },
                               { type: "science", score: 60 },
                               { type: "english", score: 88 }] }
])

// inventory collection — array of numeric ratings per item
db.inventory.insertMany([
  { item: "Laptop",  ratings: [5, 4, 5, 3, 4, 5, 2, 4], qty: 10 },
  { item: "Phone",   ratings: [4, 3, 4, 5, 4, 3],        qty: 25 },
  { item: "Tablet",  ratings: [2, 3, 2, 4, 3, 5, 1, 3],  qty: 8  }
])
```

**Expected Output for both inserts:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... } }
```

---

### 1. `$` Projection Operator — First Matching Array Element

The `$` operator returns only the **first element** in an array that matches the query filter condition.

```js
// Return the first exam element where score >= 90 (for each matching student)
db.scores.find(
  { "exams.score": { $gte: 90 } },    // query — must reference the array field
  { student: 1, "exams.$": 1 }        // projection — $ returns first matching element
)
```

**Expected Output:**
```
{ _id: ObjectId('...'), student: 'Alice', exams: [ { type: 'science', score: 92 } ] }
{ _id: ObjectId('...'), student: 'Carol', exams: [ { type: 'math',    score: 95 } ] }
```
> Bob has no exam with score ≥ 90, so he is excluded entirely.
> For Alice, the first matching exam is `science` (score 92) — `math` (score 88) did not satisfy the condition.
> For Carol, the first matching exam is `math` (score 95).

> **Rule:** `$` always returns exactly ONE array element — the first one that matched the query filter.

---

### 2. `$elemMatch` Projection — First Element Matching Multiple Criteria

`$elemMatch` in a projection returns the **first array element** that satisfies **all** conditions you specify — and those conditions can be completely independent of the main query filter.

```js
// Return the first exam where type = 'math' AND score > 80 (for every student)
db.scores.find(
  {},
  {
    student: 1,
    exams: { $elemMatch: { type: "math", score: { $gt: 80 } } },
    _id: 0
  }
)
```

**Expected Output:**
```
{ student: 'Alice', exams: [ { type: 'math', score: 88 } ] }
{ student: 'Carol', exams: [ { type: 'math', score: 95 } ] }
```
> Bob's math score is 65 (not > 80), so the `exams` field is omitted entirely from Bob's result document.
> Note: Bob's document still appears but without the `exams` key.

**Difference between `$` and `$elemMatch` in projection:**

| Feature                     | `$`                               | `$elemMatch`                         |
|-----------------------------|-----------------------------------|--------------------------------------|
| Condition source            | Must come from the query filter   | You specify conditions independently |
| Multiple conditions allowed | No                                | Yes                                  |
| Query must reference array? | Yes                               | No                                   |

---

### 3. `$slice` Projection — Return a Portion of an Array

`$slice` lets you return only some elements from an array field.

```js
// Return only the FIRST 3 ratings for each item
db.inventory.find(
  {},
  { item: 1, ratings: { $slice: 3 }, _id: 0 }
)
```

**Expected Output:**
```
{ item: 'Laptop', ratings: [ 5, 4, 5 ] }
{ item: 'Phone',  ratings: [ 4, 3, 4 ] }
{ item: 'Tablet', ratings: [ 2, 3, 2 ] }
```

---

```js
// Return only the LAST 2 ratings (negative number counts from the end)
db.inventory.find(
  {},
  { item: 1, ratings: { $slice: -2 }, _id: 0 }
)
```

**Expected Output:**
```
{ item: 'Laptop', ratings: [ 2, 4 ] }
{ item: 'Phone',  ratings: [ 4, 3 ] }
{ item: 'Tablet', ratings: [ 1, 3 ] }
```

---

```js
// $slice: [skip, limit] — skip the first 2, then return the next 3
db.inventory.find(
  {},
  { item: 1, ratings: { $slice: [2, 3] }, _id: 0 }
)
```

**Expected Output:**
```
{ item: 'Laptop', ratings: [ 5, 3, 4 ] }
{ item: 'Phone',  ratings: [ 4, 5, 4 ] }
{ item: 'Tablet', ratings: [ 2, 4, 3 ] }
```
> For Laptop: full array is [5,4,5,3,4,5,2,4]. Skip first 2 → [5,3,4,5,2,4]. Take next 3 → [5,3,4].
> For Phone: full array is [4,3,4,5,4,3]. Skip first 2 → [4,5,4,3]. Take next 3 → [4,5,4].
> For Tablet: full array is [2,3,2,4,3,5,1,3]. Skip first 2 → [2,4,3,5,1,3]. Take next 3 → [2,4,3].

---

**Viva Questions:**
1. What is the difference between `$elemMatch` as a query operator and as a projection operator?
2. When would you use `$slice` over `limit()`?
3. Can `$` return more than one matching array element?
4. What happens if no array element matches the `$elemMatch` projection condition?

---

---

# EXPERIMENT 5

## Aggregation Operations — `$avg`, `$min`, `$max`, `$push`, `$addToSet`, etc.

**Aim:** Execute aggregation operations using various aggregation operators and demonstrate multiple queries.

---

### What is Aggregation?

Aggregation processes multiple documents and returns computed results — similar to SQL `GROUP BY` with aggregate functions. The main function is:

```js
db.collection.aggregate([ stage1, stage2, ... ])
```

This experiment focuses on **accumulator operators** used inside the `$group` stage.

| Operator    | Description                                       |
|-------------|---------------------------------------------------|
| `$sum`      | Sum of values (use `$sum: 1` to count documents)  |
| `$avg`      | Average of values                                 |
| `$min`      | Minimum value                                     |
| `$max`      | Maximum value                                     |
| `$push`     | Appends values to an array (allows duplicates)    |
| `$addToSet` | Appends unique values to an array (no duplicates) |
| `$first`    | First value in the group                          |
| `$last`     | Last value in the group                           |

---

### Queries on the `students` Collection

#### 1. `$avg` — Average marks per grade

```js
db.students.aggregate([
  {
    $group: {
      _id: "$grade",
      averageMarks: { $avg: "$marks" }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'A', averageMarks: 91.67 }
{ _id: 'B', averageMarks: 77.67 }
{ _id: 'C', averageMarks: 62.5  }
```
> Grade A: Alice(92) + Charlie(88) + Frank(95) = 275 ÷ 3 = 91.67
> Grade B: Bob(78) + Eve(74) + Henry(81) = 233 ÷ 3 = 77.67
> Grade C: Diana(65) + Grace(60) = 125 ÷ 2 = 62.5

---

#### 2. `$min` and `$max` — Minimum and maximum fees per city

```js
db.students.aggregate([
  {
    $group: {
      _id: "$city",
      minFees: { $min: "$fees" },
      maxFees: { $max: "$fees" }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'Mumbai',  minFees: 40000, maxFees: 45000 }
{ _id: 'Delhi',   minFees: 38000, maxFees: 48000 }
{ _id: 'Pune',    minFees: 28000, maxFees: 30000 }
{ _id: 'Chennai', minFees: 36000, maxFees: 36000 }
```
> Mumbai: Alice(45000), Charlie(42000), Henry(40000) → min=40000, max=45000
> Delhi: Bob(38000), Frank(48000) → min=38000, max=48000
> Pune: Diana(30000), Grace(28000) → min=28000, max=30000
> Chennai: Eve(36000) only → min=max=36000

---

#### 3. `$sum` — Total fees collected per subject + student count

```js
db.students.aggregate([
  {
    $group: {
      _id: "$subject",
      totalFees:    { $sum: "$fees" },
      studentCount: { $sum: 1 }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'DBMS',     totalFees: 123000, studentCount: 3 }
{ _id: 'OS',       totalFees: 114000, studentCount: 3 }
{ _id: 'Networks', totalFees:  70000, studentCount: 2 }
```
> DBMS: Alice(45000) + Diana(30000) + Frank(48000) = 123000
> OS: Bob(38000) + Eve(36000) + Henry(40000) = 114000
> Networks: Charlie(42000) + Grace(28000) = 70000

---

#### 4. `$push` — List all student names per grade (array, preserves duplicates)

```js
db.students.aggregate([
  {
    $group: {
      _id: "$grade",
      studentNames: { $push: "$name" }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'A', studentNames: [ 'Alice', 'Charlie', 'Frank' ] }
{ _id: 'B', studentNames: [ 'Bob', 'Eve', 'Henry' ] }
{ _id: 'C', studentNames: [ 'Diana', 'Grace' ] }
```

---

#### 5. `$addToSet` — Unique cities per grade (no duplicates)

```js
db.students.aggregate([
  {
    $group: {
      _id: "$grade",
      uniqueCities: { $addToSet: "$city" }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'A', uniqueCities: [ 'Mumbai', 'Delhi' ] }
{ _id: 'B', uniqueCities: [ 'Delhi', 'Chennai', 'Mumbai' ] }
{ _id: 'C', uniqueCities: [ 'Pune' ] }
```
> Grade A: Alice(Mumbai), Charlie(Mumbai), Frank(Delhi) — Mumbai appears twice in data but only once in the set.
> Grade C: Diana(Pune), Grace(Pune) — both Pune, but set returns only one 'Pune'.

> **Note:** `$addToSet` does not guarantee order. Your output order for elements inside the array may differ.

---

#### 6. `$first` and `$last` — First and last student per subject (by insertion order)

```js
db.students.aggregate([
  {
    $group: {
      _id: "$subject",
      firstStudent: { $first: "$name" },
      lastStudent:  { $last: "$name" }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'DBMS',     firstStudent: 'Alice',   lastStudent: 'Frank'  }
{ _id: 'OS',       firstStudent: 'Bob',     lastStudent: 'Henry'  }
{ _id: 'Networks', firstStudent: 'Charlie', lastStudent: 'Grace'  }
```
> DBMS students in insertion order: Alice, Diana, Frank → first=Alice, last=Frank
> OS students: Bob, Eve, Henry → first=Bob, last=Henry
> Networks students: Charlie, Grace → first=Charlie, last=Grace

---

### Queries on the `employees` Collection

#### 7. Average, min, max salary and headcount per department

```js
db.employees.aggregate([
  {
    $group: {
      _id: "$dept",
      avgSalary: { $avg: "$salary" },
      minSalary: { $min: "$salary" },
      maxSalary: { $max: "$salary" },
      headcount:  { $sum: 1 }
    }
  }
])
```

**Expected Output:**
```
{ _id: 'HR',        avgSalary: 43500, minSalary: 42000, maxSalary: 45000, headcount: 2 }
{ _id: 'IT',        avgSalary: 84000, minSalary: 72000, maxSalary: 95000, headcount: 3 }
{ _id: 'Finance',   avgSalary: 57500, minSalary: 55000, maxSalary: 60000, headcount: 2 }
{ _id: 'Marketing', avgSalary: 48000, minSalary: 48000, maxSalary: 48000, headcount: 1 }
```
> HR: Raj(45000) + Karan(42000) → avg=43500, min=42000, max=45000
> IT: Priya(72000) + Sneha(85000) + Meena(95000) → avg=84000, min=72000, max=95000
> Finance: Anil(60000) + Suresh(55000) → avg=57500, min=55000, max=60000

---

#### 8. All skills listed in the IT department using `$push`

```js
db.employees.aggregate([
  { $match: { dept: "IT" } },
  {
    $group: {
      _id: "$dept",
      allSkills: { $push: "$skills" }
    }
  }
])
```

**Expected Output:**
```
{
  _id: 'IT',
  allSkills: [
    [ 'python', 'mongodb', 'docker' ],
    [ 'java', 'mongodb', 'kubernetes' ],
    [ 'python', 'ml', 'tensorflow' ]
  ]
}
```
> `$push` on an array field pushes the **entire array** as one element — resulting in an array of arrays.
> To flatten it, use `$unwind` on skills first (covered in Experiment 6).

---

#### 9. Unique departments where salary > 50000 using `$addToSet`

```js
db.employees.aggregate([
  { $match: { salary: { $gt: 50000 } } },
  {
    $group: {
      _id: null,
      highSalaryDepts: { $addToSet: "$dept" }
    }
  }
])
```

**Expected Output:**
```
{ _id: null, highSalaryDepts: [ 'IT', 'Finance' ] }
```
> Salary > 50000: Priya(IT,72000), Anil(Finance,60000), Sneha(IT,85000), Meena(IT,95000), Suresh(Finance,55000).
> Unique departments: IT, Finance.
> `_id: null` groups ALL matching documents into one single result.

---

**Viva Questions:**
1. What is the difference between `$push` and `$addToSet`?
2. What does `_id: null` mean in a `$group` stage?
3. How is `$sum: 1` used to count documents?
4. Can you use `$avg` on a non-numeric field? What happens?

---

---

# EXPERIMENT 6

## Aggregation Pipeline — `$match`, `$group`, `$sort`, `$project`, `$skip`, `$limit`, `$unwind`

**Aim:** Execute aggregation pipeline operations. The pipeline must contain `$match`, `$group`, `$sort`, `$project`, `$skip`, and other stages.

---

### What is an Aggregation Pipeline?

A pipeline is a **sequence of stages**. Documents flow through each stage, and each stage transforms or filters the data.

```
Input → [$match] → [$group] → [$sort] → [$project] → [$skip/$limit] → Output
```

### Pipeline Stages Reference

| Stage      | Description                                              |
|------------|----------------------------------------------------------|
| `$match`   | Filter documents (like `WHERE` in SQL)                   |
| `$group`   | Group documents and apply accumulator operators          |
| `$sort`    | Sort documents (`1` = ascending, `-1` = descending)      |
| `$project` | Reshape documents — include, exclude, rename, compute    |
| `$skip`    | Skip the first N documents                               |
| `$limit`   | Keep only the first N documents                          |
| `$unwind`  | Deconstruct an array — one document per array element    |
| `$count`   | Count documents passing through the stage                |

---

### Pipeline 1 — Full pipeline: match → group → sort → project → skip → limit

```js
// Find the 2nd and 3rd highest-paying departments (skip the top one)
db.employees.aggregate([

  // Stage 1: $match — include all four departments
  { $match: { dept: { $in: ["IT", "Finance", "HR", "Marketing"] } } },

  // Stage 2: $group — group by dept, compute avg salary and headcount
  {
    $group: {
      _id: "$dept",
      avgSalary:  { $avg: "$salary" },
      totalStaff: { $sum: 1 }
    }
  },

  // Stage 3: $sort — sort by avgSalary descending
  { $sort: { avgSalary: -1 } },

  // Stage 4: $project — rename _id to department, round avgSalary to 2 decimal places
  {
    $project: {
      _id: 0,
      department: "$_id",
      avgSalary:  { $round: ["$avgSalary", 2] },
      totalStaff: 1
    }
  },

  // Stage 5: $skip — skip the top result (IT with highest avg salary)
  { $skip: 1 },

  // Stage 6: $limit — show only the next 2 results
  { $limit: 2 }
])
```

**Expected Output:**
```
{ department: 'Finance',   avgSalary: 57500, totalStaff: 2 }
{ department: 'Marketing', avgSalary: 48000, totalStaff: 1 }
```
> After grouping and sorting: IT(84000), Finance(57500), Marketing(48000), HR(43500).
> After `$skip: 1`: Finance, Marketing, HR remain.
> After `$limit: 2`: Finance and Marketing are returned.

---

### Pipeline 2 — `$unwind` on array fields

`$unwind` "explodes" an array — each element of the array becomes a **separate document**.

```js
// See the effect of $unwind on the tags array in products
db.products.aggregate([
  { $unwind: "$tags" },
  { $project: { _id: 0, name: 1, tag: "$tags" } }
])
```

**Expected Output:**
```
{ name: 'Laptop',     tag: 'portable'  }
{ name: 'Laptop',     tag: 'computing' }
{ name: 'Phone',      tag: 'mobile'    }
{ name: 'Phone',      tag: 'computing' }
{ name: 'Desk',       tag: 'office'    }
{ name: 'Desk',       tag: 'wooden'    }
{ name: 'Chair',      tag: 'office'    }
{ name: 'Chair',      tag: 'ergonomic' }
{ name: 'Headphones', tag: 'audio'     }
{ name: 'Headphones', tag: 'portable'  }
{ name: 'Tablet',     tag: 'portable'  }
{ name: 'Tablet',     tag: 'computing' }
{ name: 'Monitor',    tag: 'display'   }
{ name: 'Monitor',    tag: 'computing' }
{ name: 'Bookshelf',  tag: 'wooden'    }
{ name: 'Bookshelf',  tag: 'storage'   }
```
> 8 products × ~2 tags each = 16 documents total.

---

#### Count products per tag using `$unwind` + `$group`

```js
db.products.aggregate([
  { $unwind: "$tags" },
  {
    $group: {
      _id: "$tags",
      count: { $sum: 1 }
    }
  },
  { $sort: { count: -1 } }
])
```

**Expected Output:**
```
{ _id: 'computing', count: 4 }
{ _id: 'portable',  count: 3 }
{ _id: 'office',    count: 2 }
{ _id: 'wooden',    count: 2 }
{ _id: 'mobile',    count: 1 }
{ _id: 'ergonomic', count: 1 }
{ _id: 'audio',     count: 1 }
{ _id: 'display',   count: 1 }
{ _id: 'storage',   count: 1 }
```
> computing: Laptop, Phone, Tablet, Monitor = 4
> portable: Laptop, Headphones, Tablet = 3
> office: Desk, Chair = 2 | wooden: Desk, Bookshelf = 2

---

### Pipeline 3 — `$project` with computed fields using `$cond`

```js
// Add a grade_label computed field based on marks
db.students.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      marks: 1,
      grade_label: {
        $cond: {
          if:   { $gte: ["$marks", 85] },
          then: "Distinction",
          else: {
            $cond: {
              if:   { $gte: ["$marks", 70] },
              then: "First Class",
              else: "Pass / Fail"
            }
          }
        }
      }
    }
  }
])
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92, grade_label: 'Distinction' }
{ name: 'Bob',     marks: 78, grade_label: 'First Class' }
{ name: 'Charlie', marks: 88, grade_label: 'Distinction' }
{ name: 'Diana',   marks: 65, grade_label: 'Pass / Fail' }
{ name: 'Eve',     marks: 74, grade_label: 'First Class' }
{ name: 'Frank',   marks: 95, grade_label: 'Distinction' }
{ name: 'Grace',   marks: 60, grade_label: 'Pass / Fail' }
{ name: 'Henry',   marks: 81, grade_label: 'First Class' }
```
> Marks ≥ 85 → Distinction: Alice(92), Charlie(88), Frank(95)
> Marks 70–84 → First Class: Bob(78), Eve(74), Henry(81)
> Marks < 70 → Pass / Fail: Diana(65), Grace(60)

---

### Pipeline 4 — Comprehensive multi-stage pipeline on students collection

```js
db.students.aggregate([

  // Stage 1: Only A and B grade students
  { $match: { grade: { $in: ["A", "B"] } } },

  // Stage 2: Group by city — avg marks, count, list of names
  {
    $group: {
      _id: "$city",
      avgMarks:     { $avg: "$marks" },
      studentCount: { $sum: 1 },
      names:        { $push: "$name" }
    }
  },

  // Stage 3: Only cities that have more than 1 student (from A/B grade)
  { $match: { studentCount: { $gt: 1 } } },

  // Stage 4: Sort by avgMarks descending
  { $sort: { avgMarks: -1 } },

  // Stage 5: Shape the output — rename _id to city, round avgMarks
  {
    $project: {
      _id: 0,
      city:         "$_id",
      avgMarks:     { $round: ["$avgMarks", 1] },
      studentCount: 1,
      names:        1
    }
  }
])
```

**Expected Output:**
```
{ city: 'Mumbai', avgMarks: 87.0, studentCount: 3, names: [ 'Alice', 'Charlie', 'Henry' ] }
{ city: 'Delhi',  avgMarks: 86.5, studentCount: 2, names: [ 'Bob', 'Frank' ] }
```
> A/B grade students: Alice(A,Mumbai,92), Bob(B,Delhi,78), Charlie(A,Mumbai,88), Eve(B,Chennai,74), Frank(A,Delhi,95), Henry(B,Mumbai,81).
> Mumbai group: Alice+Charlie+Henry → avg=(92+88+81)/3=87.0, count=3
> Delhi group: Bob+Frank → avg=(78+95)/2=86.5, count=2
> Chennai group: Eve only → count=1, filtered out by Stage 3.

---

### Pipeline 5 — `$count` stage

```js
// Count how many employees earn more than 60000
db.employees.aggregate([
  { $match: { salary: { $gt: 60000 } } },
  { $count: "highEarners" }
])
```

**Expected Output:**
```
{ highEarners: 3 }
```
> Priya(72000), Sneha(85000), Meena(95000) — all in IT dept, all earn > 60000.

---

**Viva Questions:**
1. What is the order of stages in an aggregation pipeline? Does order matter?
2. What does `$unwind` do when an array field is empty or missing?
3. How is `$project` in a pipeline different from projection in `find()`?
4. Can you use `$match` twice in the same pipeline? When would that be useful?

---

---

# EXPERIMENT 7

## Real-World Queries — Listings & Reviews Collection and E-Commerce Reviews Summary

**Aim:**
- Part A: Find listings with `listing_url`, `name`, `address`, `host_picture_url` where the host has a picture URL.
- Part B: Write a query to display a reviews summary using an E-commerce collection.

---

### Part A — Listings and Reviews Collection

#### Setup

```js
use listingsdb

db.listingsAndReviews.insertMany([
  {
    listing_url: "https://airbnb.com/rooms/101",
    name: "Cozy Studio in Mumbai",
    address: { street: "Marine Drive", city: "Mumbai", country: "India" },
    host: { host_id: "h1", host_name: "Alice", host_picture_url: "https://pics.example.com/alice.jpg" },
    price: 2500,
    review_scores: { review_scores_rating: 92 }
  },
  {
    listing_url: "https://airbnb.com/rooms/102",
    name: "Spacious Flat in Delhi",
    address: { street: "Connaught Place", city: "Delhi", country: "India" },
    host: { host_id: "h2", host_name: "Bob", host_picture_url: "" },
    price: 3200,
    review_scores: { review_scores_rating: 85 }
  },
  {
    listing_url: "https://airbnb.com/rooms/103",
    name: "Beach House Goa",
    address: { street: "Calangute", city: "Goa", country: "India" },
    host: { host_id: "h3", host_name: "Carol", host_picture_url: "https://pics.example.com/carol.jpg" },
    price: 4500,
    review_scores: { review_scores_rating: 97 }
  },
  {
    listing_url: "https://airbnb.com/rooms/104",
    name: "City Centre Apartment",
    address: { street: "MG Road", city: "Bangalore", country: "India" },
    host: { host_id: "h4", host_name: "David", host_picture_url: null },
    price: 2800,
    review_scores: { review_scores_rating: 78 }
  },
  {
    listing_url: "https://airbnb.com/rooms/105",
    name: "Heritage Haveli Jaipur",
    address: { street: "Hawa Mahal Road", city: "Jaipur", country: "India" },
    host: { host_id: "h5", host_name: "Eva", host_picture_url: "https://pics.example.com/eva.jpg" },
    price: 5000,
    review_scores: { review_scores_rating: 99 }
  }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '4': ObjectId('...') } }
```

---

#### Query A1 — Listings where the host has a picture URL (not null, not empty)

```js
db.listingsAndReviews.find(
  {
    "host.host_picture_url": { $exists: true, $nin: [null, ""] }
  },
  {
    _id: 0,
    listing_url: 1,
    name: 1,
    address: 1,
    "host.host_picture_url": 1
  }
)
```

**Expected Output:**
```
{
  listing_url: 'https://airbnb.com/rooms/101',
  name: 'Cozy Studio in Mumbai',
  address: { street: 'Marine Drive', city: 'Mumbai', country: 'India' },
  host: { host_picture_url: 'https://pics.example.com/alice.jpg' }
}
{
  listing_url: 'https://airbnb.com/rooms/103',
  name: 'Beach House Goa',
  address: { street: 'Calangute', city: 'Goa', country: 'India' },
  host: { host_picture_url: 'https://pics.example.com/carol.jpg' }
}
{
  listing_url: 'https://airbnb.com/rooms/105',
  name: 'Heritage Haveli Jaipur',
  address: { street: 'Hawa Mahal Road', city: 'Jaipur', country: 'India' },
  host: { host_picture_url: 'https://pics.example.com/eva.jpg' }
}
```
> Room 102 (Bob) has `host_picture_url: ""` (empty string) — excluded.
> Room 104 (David) has `host_picture_url: null` — excluded.

---

#### Query A2 — Sort by review rating, show top 3 (from listings with picture URLs only)

```js
db.listingsAndReviews.find(
  { "host.host_picture_url": { $exists: true, $nin: [null, ""] } },
  {
    _id: 0,
    name: 1,
    "address.city": 1,
    "review_scores.review_scores_rating": 1
  }
).sort({ "review_scores.review_scores_rating": -1 }).limit(3)
```

**Expected Output:**
```
{ name: 'Heritage Haveli Jaipur', address: { city: 'Jaipur' }, review_scores: { review_scores_rating: 99 } }
{ name: 'Beach House Goa',        address: { city: 'Goa'    }, review_scores: { review_scores_rating: 97 } }
{ name: 'Cozy Studio in Mumbai',  address: { city: 'Mumbai' }, review_scores: { review_scores_rating: 92 } }
```

---

### Part B — E-Commerce Reviews Summary

#### Setup

```js
use ecommercedb

db.reviews.insertMany([
  { product_id: "P001", product_name: "Laptop",     user: "Alice", rating: 5, review: "Excellent performance!", date: new Date("2024-01-10") },
  { product_id: "P001", product_name: "Laptop",     user: "Bob",   rating: 4, review: "Good but expensive.",    date: new Date("2024-01-15") },
  { product_id: "P001", product_name: "Laptop",     user: "Carol", rating: 3, review: "Average battery life.",  date: new Date("2024-02-01") },
  { product_id: "P002", product_name: "Phone",      user: "Dave",  rating: 5, review: "Amazing camera.",        date: new Date("2024-01-20") },
  { product_id: "P002", product_name: "Phone",      user: "Eve",   rating: 2, review: "Heats up quickly.",      date: new Date("2024-02-05") },
  { product_id: "P003", product_name: "Headphones", user: "Frank", rating: 5, review: "Crystal clear audio.",   date: new Date("2024-01-25") },
  { product_id: "P003", product_name: "Headphones", user: "Grace", rating: 4, review: "Comfortable fit.",       date: new Date("2024-02-10") },
  { product_id: "P003", product_name: "Headphones", user: "Henry", rating: 5, review: "Great noise cancelling.",date: new Date("2024-02-12") }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedIds: { '0': ObjectId('...'), ... '7': ObjectId('...') } }
```

---

#### Reviews Summary Query

```js
db.reviews.aggregate([

  // Stage 1: Group by product, compute all summary metrics
  {
    $group: {
      _id: "$product_id",
      productName:   { $first: "$product_name" },
      totalReviews:  { $sum: 1 },
      averageRating: { $avg: "$rating" },
      minRating:     { $min: "$rating" },
      maxRating:     { $max: "$rating" }
    }
  },

  // Stage 2: Shape output and add a sentiment label
  {
    $project: {
      _id: 0,
      productId:     "$_id",
      productName:   1,
      totalReviews:  1,
      averageRating: { $round: ["$averageRating", 2] },
      minRating:     1,
      maxRating:     1,
      sentiment: {
        $cond: {
          if:   { $gte: ["$averageRating", 4.5] }, then: "Excellent",
          else: { $cond: {
            if:   { $gte: ["$averageRating", 3.5] }, then: "Good",
            else: { $cond: {
              if:   { $gte: ["$averageRating", 2.5] }, then: "Average",
              else: "Poor"
            }}
          }}
        }
      }
    }
  },

  // Stage 3: Sort by average rating descending
  { $sort: { averageRating: -1 } }
])
```

**Expected Output:**
```
{
  productId: 'P003', productName: 'Headphones',
  totalReviews: 3, averageRating: 4.67, minRating: 4, maxRating: 5,
  sentiment: 'Excellent'
}
{
  productId: 'P001', productName: 'Laptop',
  totalReviews: 3, averageRating: 4, minRating: 3, maxRating: 5,
  sentiment: 'Good'
}
{
  productId: 'P002', productName: 'Phone',
  totalReviews: 2, averageRating: 3.5, minRating: 2, maxRating: 5,
  sentiment: 'Good'
}
```
> Headphones: (5+4+5)/3 = 14/3 = 4.67 → Excellent (≥ 4.5)
> Laptop: (5+4+3)/3 = 12/3 = 4.0 → Good (≥ 3.5)
> Phone: (5+2)/2 = 7/2 = 3.5 → Good (≥ 3.5)

---

**Viva Questions:**
1. How do you query a nested field in MongoDB?
2. What does `$exists` do in a query?
3. Why do we use `$first` inside `$group` to get the product name?
4. What does `$cond` do in an aggregation pipeline? What are the three required parts?

---

---

# EXPERIMENT 8

## Indexing — Unique, Sparse, Compound, Multikey Indexes & Query Optimization

**Aim:**
- Part A: Demonstrate creation of different types of indexes on a collection.
- Part B: Demonstrate optimization of queries using indexes.

> Switch back to labdb for this experiment: `use labdb`

---

### Why Indexing?

Without an index, MongoDB must scan **every document** in a collection (a **COLLSCAN**) to find matching documents. Indexes allow MongoDB to jump directly to relevant documents (**IXSCAN**) — dramatically improving performance.

---

### Part A — Creating Different Index Types

#### 1. Single Field Index

```js
// Create a basic ascending index on the marks field
db.students.createIndex({ marks: 1 })
```

**Expected Output:**
```
marks_1
```
> MongoDB returns the name of the newly created index.

```js
// View all indexes on the collection
db.students.getIndexes()
```

**Expected Output:**
```
[
  {
    v: 2,
    key: { _id: 1 },
    name: '_id_'
  },
  {
    v: 2,
    key: { marks: 1 },
    name: 'marks_1'
  }
]
```
> Every collection always has the default `_id` index. Our new `marks_1` index appears second.

---

#### 2. Unique Index — enforces unique values

```js
// Add an email field to all existing students first
db.students.updateMany({}, [
  { $set: { email: { $concat: [ { $toLower: "$name" }, "@college.edu" ] } } }
])
```

**Expected Output:**
```
{ acknowledged: true, insertedId: null, matchedCount: 8, modifiedCount: 8, upsertedId: null }
```

```js
// Create a unique index on email — no two students can share an email
db.students.createIndex({ email: 1 }, { unique: true })
```

**Expected Output:**
```
email_1
```

```js
// Verify emails were added
db.students.find({}, { name: 1, email: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   email: 'alice@college.edu'   }
{ name: 'Bob',     email: 'bob@college.edu'     }
{ name: 'Charlie', email: 'charlie@college.edu' }
{ name: 'Diana',   email: 'diana@college.edu'   }
{ name: 'Eve',     email: 'eve@college.edu'     }
{ name: 'Frank',   email: 'frank@college.edu'   }
{ name: 'Grace',   email: 'grace@college.edu'   }
{ name: 'Henry',   email: 'henry@college.edu'   }
```

```js
// Try inserting a DUPLICATE email — this will FAIL
db.students.insertOne({ name: "Alex", email: "alice@college.edu", marks: 90 })
```

**Expected Output (Error):**
```
MongoServerError: E11000 duplicate key error collection: labdb.students
index: email_1 dup key: { email: "alice@college.edu" }
```
> The unique index prevents duplicate values from being inserted.

---

#### 3. Sparse Index — only indexes documents where the field exists

```js
// Give scholarship only to students with marks >= 85
db.students.updateMany(
  { marks: { $gte: 85 } },
  { $set: { scholarship: "Merit" } }
)
```

**Expected Output:**
```
{ acknowledged: true, insertedId: null, matchedCount: 3, modifiedCount: 3, upsertedId: null }
```
> Alice(92), Charlie(88), Frank(95) qualify. The other 5 students have no `scholarship` field.

```js
// Create a sparse index — only documents WITH the scholarship field are indexed
db.students.createIndex({ scholarship: 1 }, { sparse: true })
```

**Expected Output:**
```
scholarship_1
```

```js
// Query using the sparse index
db.students.find({ scholarship: "Merit" }, { name: 1, marks: 1, scholarship: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   marks: 92, scholarship: 'Merit' }
{ name: 'Charlie', marks: 88, scholarship: 'Merit' }
{ name: 'Frank',   marks: 95, scholarship: 'Merit' }
```
> A regular index would store `null` for the 5 students without a scholarship field. A sparse index skips them entirely, saving space.

---

#### 4. Compound Index — index on multiple fields together

```js
// Create a compound index on city AND grade
db.students.createIndex({ city: 1, grade: 1 })
```

**Expected Output:**
```
city_1_grade_1
```

```js
// This query BENEFITS from the compound index (uses both fields)
db.students.find({ city: "Mumbai", grade: "A" }, { name: 1, city: 1, grade: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai', grade: 'A' }
{ name: 'Charlie', city: 'Mumbai', grade: 'A' }
```

```js
// Query on just the LEADING field also uses the index
db.students.find({ city: "Mumbai" }, { name: 1, city: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Alice',   city: 'Mumbai' }
{ name: 'Charlie', city: 'Mumbai' }
{ name: 'Henry',   city: 'Mumbai' }
```

> **Left-most prefix rule:** A compound index `{ a:1, b:1, c:1 }` supports queries on `a`, `a+b`, and `a+b+c` — but NOT on `b` alone or `c` alone.

---

#### 5. Multikey Index — index on array fields

```js
// MongoDB automatically detects that skills is an array and creates a multikey index
db.employees.createIndex({ skills: 1 })
```

**Expected Output:**
```
skills_1
```

```js
// Now queries on individual array elements are fast
db.employees.find({ skills: "mongodb" }, { name: 1, dept: 1, skills: 1, _id: 0 })
```

**Expected Output:**
```
{ name: 'Priya', dept: 'IT', skills: [ 'python', 'mongodb', 'docker'     ] }
{ name: 'Sneha', dept: 'IT', skills: [ 'java',   'mongodb', 'kubernetes' ] }
```
> MongoDB searches through all values in each `skills` array. Priya and Sneha both have "mongodb" in their skills.

```js
// Verify it is a multikey index by listing indexes
db.employees.getIndexes()
```

**Expected Output:**
```
[
  { v: 2, key: { _id: 1 },      name: '_id_'     },
  { v: 2, key: { skills: 1 },   name: 'skills_1' }
]
```
> When you run `explain()` on a query using this index, you will see `"isMultiKey": true` in the plan details.

---

### Part B — Query Optimization with `explain()`

The `explain()` method shows MongoDB's **query execution plan** — revealing whether it used an index or did a full collection scan.

#### Step 1: Drop the marks index and run explain (no index — COLLSCAN)

```js
db.students.dropIndex("marks_1")
```

**Expected Output:**
```
{ nIndexesWas: 4, ok: 1 }
```

```js
db.students.find({ marks: { $gt: 80 } }).explain("executionStats")
```

**Expected Output (key fields):**
```
{
  queryPlanner: {
    winningPlan: {
      stage: 'COLLSCAN',     ← Full Collection Scan — no index used
      ...
    }
  },
  executionStats: {
    nReturned: 4,            ← 4 documents matched (Alice, Charlie, Frank, Henry)
    totalDocsExamined: 8,    ← Had to scan ALL 8 documents
    totalKeysExamined: 0,    ← No index keys examined
    executionTimeMillis: 1
  }
}
```

---

#### Step 2: Create the marks index and run explain again (IXSCAN)

```js
db.students.createIndex({ marks: 1 })
```

**Expected Output:**
```
marks_1
```

```js
db.students.find({ marks: { $gt: 80 } }).explain("executionStats")
```

**Expected Output (key fields):**
```
{
  queryPlanner: {
    winningPlan: {
      stage: 'FETCH',
      inputStage: {
        stage: 'IXSCAN',   ← Index Scan — index was used ✅
        keyPattern: { marks: 1 },
        indexName: 'marks_1'
      }
    }
  },
  executionStats: {
    nReturned: 4,           ← Same 4 documents returned
    totalDocsExamined: 4,   ← Only 4 docs examined (not all 8) ✅
    totalKeysExamined: 4,   ← 4 index keys examined
    executionTimeMillis: 0
  }
}
```

#### Key `explain()` fields to check

| Field                  | What it means                                          |
|------------------------|--------------------------------------------------------|
| `stage: "COLLSCAN"`    | Full collection scan — no index used (slow)            |
| `stage: "IXSCAN"`      | Index scan — index was used ✅ (fast)                   |
| `nReturned`            | Number of documents returned to the client             |
| `totalDocsExamined`    | Total documents physically read (lower is better)      |
| `totalKeysExamined`    | Total index entries scanned (lower is better)          |
| `executionTimeMillis`  | Time taken in milliseconds (lower is better)           |

---

#### Viewing and dropping indexes

```js
// List all current indexes on students
db.students.getIndexes()
```

**Expected Output:**
```
[
  { v: 2, key: { _id: 1 },          name: '_id_'            },
  { v: 2, key: { email: 1 },        name: 'email_1',        unique: true },
  { v: 2, key: { scholarship: 1 },  name: 'scholarship_1',  sparse: true },
  { v: 2, key: { city: 1, grade: 1},name: 'city_1_grade_1'  },
  { v: 2, key: { marks: 1 },        name: 'marks_1'         }
]
```

```js
// Drop a specific index by name
db.students.dropIndex("marks_1")
```

**Expected Output:**
```
{ nIndexesWas: 5, ok: 1 }
```

```js
// Drop ALL non-_id indexes at once
db.students.dropIndexes()
```

**Expected Output:**
```
{ nIndexesWas: 4, msg: 'non-_id indexes dropped for collection', ok: 1 }
```

---

**Viva Questions:**
1. What is the difference between a sparse index and a regular index?
2. What is the left-most prefix rule for compound indexes?
3. What is a multikey index? When is it automatically created?
4. What do `COLLSCAN` and `IXSCAN` mean in an `explain()` output?
5. Can you have a unique index on a field that has `null` values? How does a sparse index help?

---

---

# EXPERIMENT 9

## Text Search — Full Text Search & Excluding Documents

**Aim:**
- Part A: Develop a query to demonstrate text search using catalog data collection for a given word.
- Part B: Develop queries to illustrate excluding documents with certain words and phrases.

> Switch back to labdb: `use labdb`

---

### Background

MongoDB's **text index** enables full-text search on string fields. It supports single-word search, multi-word search (OR by default), phrase search (exact match), exclusion of words, and relevance scoring.

---

### Step 1 — Create a Text Index

```js
// Create a text index covering both title and description
db.catalog.createIndex({ title: "text", description: "text" })
```

**Expected Output:**
```
title_text_description_text
```

```js
// Verify the index was created
db.catalog.getIndexes()
```

**Expected Output:**
```
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  {
    v: 2,
    key: { _fts: 'text', _ftsx: 1 },
    name: 'title_text_description_text',
    weights: { description: 1, title: 1 },
    default_language: 'english',
    language_override: 'language',
    textIndexVersion: 3
  }
]
```

> **Important:** A collection can have **only one text index**. It can cover multiple fields in a single index definition.

---

### Part A — Basic Text Search

Text search uses the `$text` operator combined with `$search`.

#### 1. Single-word search

```js
// Find all documents containing the word "MongoDB" (case-insensitive)
db.catalog.find(
  { $text: { $search: "MongoDB" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB' }
{ title: 'Indexing in MongoDB'     }
{ title: 'Python with MongoDB'     }
{ title: 'MongoDB Security Guide'  }
{ title: 'Full Text Search MongoDB'}
```
> "Advanced Aggregation" — no "MongoDB" → excluded.
> "Data Modeling NoSQL" — no "MongoDB" → excluded.
> "Replication and Sharding" — no "MongoDB" → excluded.
> Text search is **case-insensitive**: "mongodb", "MongoDB", "MONGODB" all match.

---

#### 2. Multi-word search (OR logic — a document matches if it contains ANY of the words)

```js
// Find documents containing "aggregation" OR "pipeline"
db.catalog.find(
  { $text: { $search: "aggregation pipeline" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Advanced Aggregation' }
```
> Only "Advanced Aggregation" contains "aggregation" in both its title and description ("aggregation pipelines"). No other document contains either word.

---

#### 3. Search with relevance score

```js
// Sort by relevance — most relevant document appears first
db.catalog.find(
  { $text: { $search: "MongoDB database" } },
  { title: 1, score: { $meta: "textScore" }, _id: 0 }
).sort({ score: { $meta: "textScore" } })
```

**Expected Output (approximate — scores will vary):**
```
{ title: 'Introduction to MongoDB', score: 1.5  }
{ title: 'Indexing in MongoDB',     score: 0.75 }
{ title: 'Python with MongoDB',     score: 0.75 }
{ title: 'MongoDB Security Guide',  score: 0.75 }
{ title: 'Full Text Search MongoDB',score: 0.75 }
{ title: 'Data Modeling NoSQL',     score: 0.5  }
```
> "Introduction to MongoDB" scores highest because its description also contains the word "database" — it matches both search terms. "Data Modeling NoSQL" matches only "database" (in its description) but not "MongoDB".

> **Note:** Exact score values depend on MongoDB's internal TF-IDF algorithm and may vary slightly.

---

#### 4. Phrase search — exact multi-word phrase (use escaped quotes inside `$search`)

```js
// Find documents with the exact phrase "full text search"
db.catalog.find(
  { $text: { $search: "\"full text search\"" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Full Text Search MongoDB' }
```
> Only this document contains all three words "full", "text", "search" as a consecutive phrase. The description of this document also contains "full text search" — confirming the match.

---

### Part B — Excluding Documents with Certain Words and Phrases

To **exclude** documents containing a specific word, prefix it with a **minus sign (`-`)** inside `$search`.

#### 1. Search for "MongoDB" but EXCLUDE documents mentioning "Python"

```js
db.catalog.find(
  { $text: { $search: "MongoDB -Python" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB' }
{ title: 'Indexing in MongoDB'     }
{ title: 'MongoDB Security Guide'  }
{ title: 'Full Text Search MongoDB'}
```
> "Python with MongoDB" is excluded because its title contains "Python".

---

#### 2. Search for "search" but EXCLUDE documents mentioning "indexing"

```js
db.catalog.find(
  { $text: { $search: "search -indexing" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Full Text Search MongoDB' }
```
> "Full Text Search MongoDB" contains "search" and does not contain "indexing" → included.
> "Indexing in MongoDB" does not contain "search" → already excluded.

---

#### 3. Exclude an exact phrase

```js
// Find all MongoDB documents but exclude those about "replication" and "sharding"
db.catalog.find(
  { $text: { $search: "MongoDB -\"replication sharding\"" } },
  { title: 1, _id: 0 }
)
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB' }
{ title: 'Indexing in MongoDB'     }
{ title: 'Python with MongoDB'     }
{ title: 'MongoDB Security Guide'  }
{ title: 'Full Text Search MongoDB'}
```
> "Replication and Sharding" is excluded (contains both "replication" and "sharding" as a phrase).

---

#### 4. Combine multiple inclusions and exclusions

```js
// Find documents about "database" or "NoSQL", but exclude those about "security" or "python"
db.catalog.find(
  { $text: { $search: "database NoSQL -security -python" } },
  { title: 1, score: { $meta: "textScore" }, _id: 0 }
).sort({ score: { $meta: "textScore" } })
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB', score: 1.0  }
{ title: 'Data Modeling NoSQL',     score: 0.75 }
```
> Matches "database" or "NoSQL": Introduction to MongoDB (description has "database" and "NoSQL"), Data Modeling NoSQL (title has "NoSQL", description has "NoSQL databases").
> "MongoDB Security Guide" would match "database" concept but contains "security" → excluded.
> "Python with MongoDB" contains "python" → excluded.

---

#### 5. Count matching documents

```js
// How many catalog documents mention "MongoDB"?
db.catalog.countDocuments({ $text: { $search: "MongoDB" } })
```

**Expected Output:**
```
5
```
> Introduction to MongoDB, Indexing in MongoDB, Python with MongoDB, MongoDB Security Guide, Full Text Search MongoDB.

---

### Text Search Quick Reference

| `$search` string                | Behaviour                                               |
|---------------------------------|---------------------------------------------------------|
| `"mongodb"`                     | Docs containing "mongodb" (case insensitive)            |
| `"mongodb aggregation"`         | Docs with "mongodb" OR "aggregation"                    |
| `"\"full text search\""`        | Docs with the exact phrase "full text search"           |
| `"mongodb -python"`             | Docs with "mongodb" but NOT "python"                    |
| `"database -\"security guide\""` | Docs with "database", excluding exact phrase            |

---

**Viva Questions:**
1. Can a collection have more than one text index?
2. What is `$meta: "textScore"` and how is it used?
3. How do you search for an exact phrase in MongoDB text search?
4. How does the `-` prefix work in `$search`?
5. Is text search in MongoDB case-sensitive?

---

---

# EXPERIMENT 10

## Aggregation Pipeline with Text Search on the Catalog Collection

**Aim:** Develop aggregation pipelines that combine `$text` search with pipeline stages such as `$match`, `$project`, `$sort`, `$group`, `$limit`, and `$skip` to perform advanced text-based analysis on the catalog collection.

> **Prerequisites:** The `catalog` collection must exist with the text index already created from Experiment 9. If not, run:
> ```js
> use labdb
> db.catalog.createIndex({ title: "text", description: "text" })
> ```

---

### Background — Why combine Text Search with Aggregation?

A plain `$text` query in `find()` returns matching documents. Wrapping that same filter inside an aggregation pipeline's `$match` stage lets you then **transform, count, group, score, and slice** those results using the full power of the pipeline — something `find()` alone cannot do.

```
[$match with $text] → [$addFields textScore] → [$sort] → [$project] → [$skip/$limit]
```

---

### Pipeline 1 — Basic: match by keyword → project with score → sort by relevance

```js
// Search for "MongoDB", attach a relevance score, sort best matches first
db.catalog.aggregate([

  // Stage 1: $match — filter documents containing the word "MongoDB"
  { $match: { $text: { $search: "MongoDB" } } },

  // Stage 2: $addFields — attach the text relevance score as a new field
  { $addFields: { relevanceScore: { $meta: "textScore" } } },

  // Stage 3: $project — shape the output, drop _id
  {
    $project: {
      _id: 0,
      title: 1,
      description: 1,
      relevanceScore: 1
    }
  },

  // Stage 4: $sort — best match first
  { $sort: { relevanceScore: -1 } }
])
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB',  description: 'A beginner guide to MongoDB database and NoSQL concepts.',                         relevanceScore: 1.5  }
{ title: 'Indexing in MongoDB',      description: 'Explore how indexes improve query performance in MongoDB.',                        relevanceScore: 0.75 }
{ title: 'Python with MongoDB',      description: 'Connect Python applications to MongoDB using PyMongo driver.',                    relevanceScore: 0.75 }
{ title: 'MongoDB Security Guide',   description: 'Authentication, authorization, and encryption in MongoDB.',                       relevanceScore: 0.75 }
{ title: 'Full Text Search MongoDB', description: 'Implement full text search and relevance scoring using MongoDB Atlas.',           relevanceScore: 0.75 }
```
> "Introduction to MongoDB" scores highest (1.5) because "MongoDB" appears in both the title AND the description — both indexed fields contribute to the score. All others score 0.75 (one occurrence, in one field only).

---

### Pipeline 2 — Keyword search with `$limit` and `$skip` (paginated results)

```js
// Page 1: top 3 results for the word "MongoDB"
db.catalog.aggregate([
  { $match: { $text: { $search: "MongoDB" } } },
  { $addFields: { relevanceScore: { $meta: "textScore" } } },
  { $sort: { relevanceScore: -1 } },
  { $skip: 0 },    // Page 1: skip 0
  { $limit: 3 },
  { $project: { _id: 0, title: 1, relevanceScore: 1 } }
])
```

**Expected Output (Page 1):**
```
{ title: 'Introduction to MongoDB',  relevanceScore: 1.5  }
{ title: 'Indexing in MongoDB',      relevanceScore: 0.75 }
{ title: 'Python with MongoDB',      relevanceScore: 0.75 }
```

```js
// Page 2: next 3 results (skip the first 3)
db.catalog.aggregate([
  { $match: { $text: { $search: "MongoDB" } } },
  { $addFields: { relevanceScore: { $meta: "textScore" } } },
  { $sort: { relevanceScore: -1 } },
  { $skip: 3 },    // Page 2: skip the first 3
  { $limit: 3 },
  { $project: { _id: 0, title: 1, relevanceScore: 1 } }
])
```

**Expected Output (Page 2):**
```
{ title: 'MongoDB Security Guide',   relevanceScore: 0.75 }
{ title: 'Full Text Search MongoDB', relevanceScore: 0.75 }
```
> Only 2 documents remain after skipping 3. `$limit: 3` returns however many are left.

---

### Pipeline 3 — Count matching documents using `$count`

```js
// How many catalog entries mention "MongoDB" or "NoSQL"?
db.catalog.aggregate([
  { $match: { $text: { $search: "MongoDB NoSQL" } } },
  { $count: "matchingDocuments" }
])
```

**Expected Output:**
```
{ matchingDocuments: 7 }
```
> "MongoDB NoSQL" is an OR search. Documents matching "MongoDB": 5. Documents matching "NoSQL": Data Modeling NoSQL + Introduction to MongoDB (has "NoSQL" in description) — but Introduction to MongoDB already counted. Net unique matches: Introduction to MongoDB, Indexing in MongoDB, Python with MongoDB, MongoDB Security Guide, Full Text Search MongoDB, Data Modeling NoSQL = 6… plus Replication and Sharding (description has "sharding" but not explicitly NoSQL — let's count the actual matches).

> **Note:** The exact count depends on which documents' descriptions contain the words after text stemming. Run the pipeline without `$count` first to confirm which documents match, then add `$count`.

---

### Pipeline 4 — Phrase search inside a pipeline + `$project` label

```js
// Find exact phrase "full text search", add a category label
db.catalog.aggregate([

  // Stage 1: Exact phrase match
  { $match: { $text: { $search: "\"full text search\"" } } },

  // Stage 2: Add relevance score and a fixed category label
  {
    $addFields: {
      relevanceScore: { $meta: "textScore" },
      category: "Search & Retrieval"
    }
  },

  // Stage 3: Shape the output
  {
    $project: {
      _id: 0,
      title: 1,
      description: 1,
      relevanceScore: 1,
      category: 1
    }
  }
])
```

**Expected Output:**
```
{
  title: 'Full Text Search MongoDB',
  description: 'Implement full text search and relevance scoring using MongoDB Atlas.',
  relevanceScore: 1.875,
  category: 'Search & Retrieval'
}
```
> The phrase "full text search" appears in the title (3 matching words) and also in the description — both contributing to a higher score than single-word searches.

---

### Pipeline 5 — Exclusion search inside a pipeline

```js
// Find "MongoDB" documents but exclude those about "python" or "security"
db.catalog.aggregate([
  { $match: { $text: { $search: "MongoDB -python -security" } } },
  { $addFields: { relevanceScore: { $meta: "textScore" } } },
  { $sort: { relevanceScore: -1 } },
  {
    $project: {
      _id: 0,
      title: 1,
      relevanceScore: { $round: ["$relevanceScore", 2] }
    }
  }
])
```

**Expected Output:**
```
{ title: 'Introduction to MongoDB',  relevanceScore: 1.5  }
{ title: 'Indexing in MongoDB',      relevanceScore: 0.75 }
{ title: 'Full Text Search MongoDB', relevanceScore: 0.75 }
```
> "Python with MongoDB" excluded (contains "python").
> "MongoDB Security Guide" excluded (contains "security").

---

### Pipeline 6 — Group by keyword presence: classify catalog entries

```js
// Classify each "MongoDB" document as 'Advanced' (score >= 1.0) or 'Introductory'
db.catalog.aggregate([

  // Stage 1: Match documents about MongoDB
  { $match: { $text: { $search: "MongoDB" } } },

  // Stage 2: Attach score
  { $addFields: { relevanceScore: { $meta: "textScore" } } },

  // Stage 3: Add a difficulty classification based on relevance score
  {
    $addFields: {
      difficulty: {
        $cond: {
          if:   { $gte: ["$relevanceScore", 1.0] },
          then: "Core / Introductory",
          else: "Supplementary"
        }
      }
    }
  },

  // Stage 4: Group by difficulty, collect titles
  {
    $group: {
      _id: "$difficulty",
      count:  { $sum: 1 },
      titles: { $push: "$title" }
    }
  },

  // Stage 5: Rename _id and sort
  {
    $project: {
      _id: 0,
      difficulty: "$_id",
      count: 1,
      titles: 1
    }
  },
  { $sort: { count: -1 } }
])
```

**Expected Output:**
```
{
  difficulty: 'Supplementary',
  count: 4,
  titles: [
    'Indexing in MongoDB',
    'Python with MongoDB',
    'MongoDB Security Guide',
    'Full Text Search MongoDB'
  ]
}
{
  difficulty: 'Core / Introductory',
  count: 1,
  titles: [ 'Introduction to MongoDB' ]
}
```
> "Introduction to MongoDB" scores 1.5 (≥ 1.0) → classified as Core / Introductory.
> The other four MongoDB documents score 0.75 (< 1.0) → classified as Supplementary.

---

### Pipeline 7 — Multi-word OR search with `$group` to summarise by first word of title

```js
// Search for "database" or "search", then group results by their first title word
db.catalog.aggregate([

  // Stage 1: Match any document mentioning "database" or "search"
  { $match: { $text: { $search: "database search" } } },

  // Stage 2: Extract the first word of the title as a topic key
  {
    $addFields: {
      topicKey: { $arrayElemAt: [ { $split: ["$title", " "] }, 0 ] },
      relevanceScore: { $meta: "textScore" }
    }
  },

  // Stage 3: Sort by relevance
  { $sort: { relevanceScore: -1 } },

  // Stage 4: Project final readable output
  {
    $project: {
      _id: 0,
      title: 1,
      topicKey: 1,
      relevanceScore: { $round: ["$relevanceScore", 2] }
    }
  }
])
```

**Expected Output:**
```
{ title: 'Full Text Search MongoDB', topicKey: 'Full',         relevanceScore: 1.5  }
{ title: 'Introduction to MongoDB',  topicKey: 'Introduction', relevanceScore: 0.75 }
{ title: 'Data Modeling NoSQL',      topicKey: 'Data',         relevanceScore: 0.5  }
```
> "Full Text Search MongoDB": "search" appears in title AND description → highest score.
> "Introduction to MongoDB": "database" appears in description ("MongoDB database") → matched.
> "Data Modeling NoSQL": "database" appears in description ("NoSQL databases") → matched.

---

### Summary — Text Search inside Aggregation vs. inside find()

| Feature                         | `find()` with `$text`    | `aggregate()` with `$match: {$text}` |
|---------------------------------|--------------------------|--------------------------------------|
| Filter by keyword               | ✅ Yes                    | ✅ Yes                                |
| Sort by relevance score         | ✅ `.sort({$meta})`       | ✅ Via `$addFields` + `$sort`         |
| Limit / paginate results        | ✅ `.limit()` / `.skip()` | ✅ `$limit` / `$skip` stages          |
| Group / count results           | ❌ Not possible           | ✅ `$group` / `$count` stages         |
| Add computed fields             | ❌ Not possible           | ✅ `$addFields` / `$project`          |
| Classify by score               | ❌ Not possible           | ✅ `$cond` in `$addFields`            |

---

**Viva Questions:**
1. Why can't you use `db.collection.group()` directly on text search results without aggregation?
2. Where in an aggregation pipeline must the `$text` operator appear, and why?
3. What does `$addFields: { score: { $meta: "textScore" } }` do? How is it different from including `$meta` only in `$project`?
4. If you use `$sort: { relevanceScore: -1 }` after `$addFields`, does the pipeline still use the text index efficiently? Explain.
5. How would you modify Pipeline 6 to also include a filter that only keeps documents with a relevance score above 0.5?

---

---

## Quick Reference — Common `mongosh` Commands

```js
show dbs                          // List all databases
use <dbname>                      // Switch to (or create) a database
show collections                  // List all collections in current database
db.dropDatabase()                 // Drop the current database

db.<col>.find()                   // Return all documents
db.<col>.find().pretty()          // Formatted output (older mongosh versions)
db.<col>.countDocuments()         // Count all documents
db.<col>.distinct("field")        // Get unique values of a field
db.<col>.drop()                   // Drop a collection

db.<col>.createIndex({ field: 1 })         // Create ascending index
db.<col>.getIndexes()                      // List all indexes on a collection
db.<col>.dropIndex("index_name")           // Drop index by name
db.<col>.dropIndexes()                     // Drop all indexes except _id

db.<col>.find(<filter>).explain("executionStats")   // View query execution plan
```

---

## Common Errors & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `E11000 duplicate key error` | Inserting a duplicate value on a unique index | Use a different value or drop the unique index |
| `$text requires a text index` | No text index exists on the collection | Run `db.col.createIndex({ field: "text" })` first |
| `Cannot have more than one text index` | A text index already exists | Drop the existing text index before creating a new one |
| `$near requires a 2dsphere or 2d index` | Geospatial query without a spatial index | Run `db.col.createIndex({ loc: "2dsphere" })` first |
| `MongoNotConnectedError` | `mongosh` lost connection to the server | Restart `mongosh`; verify `mongod` service is running |
| `matchedCount: 1, modifiedCount: 0` | Document matched but was already at the target value | Not an error — the update had no change to make |

---

*End of MongoDB Lab Manual — All 10 Experiments*

---
> Prepared for Engineering 2nd Year — Database Management Systems Laboratory
