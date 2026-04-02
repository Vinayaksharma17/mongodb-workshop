**MongoDB Lab — Experiment Checklist**

- [ ] **Experiment 1** — WHERE Clause, AND/OR Operations & Basic CRUD
  - [ ] Part A — Simple WHERE, AND, OR, combined AND+OR filtering
  - [ ] Part B — insertOne / insertMany
  - [ ] Part B — find / findOne / countDocuments
  - [ ] Part B — updateOne / updateMany / replaceOne
  - [ ] Part B — deleteOne / deleteMany
  - [ ] Part B — Projection (include, exclude, filter + projection)

- [ ] **Experiment 2** — Field Selection with Projection & limit/find
  - [ ] Part A — Include specific fields
  - [ ] Part A — Exclude specific fields
  - [ ] Part A — Projection with filter condition
  - [ ] Part A — Nested field projection
  - [ ] Part B — Basic limit()
  - [ ] Part B — limit() with sort()
  - [ ] Part B — skip() for pagination
  - [ ] Part B — limit() on filtered query
  - [ ] Part B — countDocuments()

- [ ] **Experiment 3** — Comparison, Logical, Geospatial & Bitwise Selectors
  - [ ] Part A — $eq, $ne, $gt, $gte/$lte, $in, $nin
  - [ ] Part A — $and, $or, $nor, $not
  - [ ] Part B — Setup places collection + 2dsphere index
  - [ ] Part B — $near
  - [ ] Part B — $geoWithin
  - [ ] Part B — Setup permissions collection
  - [ ] Part B — $bitsAllSet, $bitsAnySet, $bitsAllClear, $bitsAnyClear

- [ ] **Experiment 4** — Projection Operators ($, $elemMatch, $slice)
  - [ ] Setup scores and inventory collections
  - [ ] $ operator — first matching array element
  - [ ] $elemMatch — first element matching multiple criteria
  - [ ] $slice — first N elements
  - [ ] $slice — last N elements
  - [ ] $slice — skip + take

- [ ] **Experiment 5** — Aggregation Accumulators
  - [ ] $avg — average marks per grade
  - [ ] $min / $max — fees per city
  - [ ] $sum — total fees + count per subject
  - [ ] $push — student names per grade
  - [ ] $addToSet — unique cities per grade
  - [ ] $first / $last — per subject
  - [ ] avg/min/max salary per department
  - [ ] $push skills in IT dept
  - [ ] $addToSet unique high-salary departments

- [ ] **Experiment 6** — Aggregation Pipeline
  - [ ] Pipeline 1 — match → group → sort → project → skip → limit
  - [ ] Pipeline 2 — $unwind on tags array
  - [ ] Pipeline 2 — count per tag with $unwind + $group
  - [ ] Pipeline 3 — $project with $cond computed fields
  - [ ] Pipeline 4 — Comprehensive multi-stage pipeline
  - [ ] Pipeline 5 — $count stage

- [ ] **Experiment 7** — Real-World Queries
  - [ ] Part A — Setup listingsAndReviews collection
  - [ ] Part A — Query listings with valid host picture URL
  - [ ] Part A — Sort by review rating + limit
  - [ ] Part B — Setup reviews collection
  - [ ] Part B — Reviews summary aggregation pipeline

- [ ] **Experiment 8** — Indexing & Query Optimization
  - [ ] Part A — Single field index + getIndexes()
  - [ ] Part A — Unique index + duplicate key error test
  - [ ] Part A — Sparse index
  - [ ] Part A — Compound index + left-most prefix rule
  - [ ] Part A — Multikey index on array field
  - [ ] Part B — explain() without index (COLLSCAN)
  - [ ] Part B — explain() with index (IXSCAN)
  - [ ] Part B — dropIndex / dropIndexes

- [ ] **Experiment 9** — Text Search & Excluding Documents
  - [ ] Setup text index on title + description
  - [ ] Part A — Single word search
  - [ ] Part A — Multi-word OR search
  - [ ] Part A — Search with relevance score ($meta: "textScore")
  - [ ] Part A — Exact phrase search
  - [ ] Part B — Exclude a word (- prefix)
  - [ ] Part B — Exclude with keyword combination
  - [ ] Part B — Exclude an exact phrase
  - [ ] Part B — Multiple inclusions and exclusions
  - [ ] Part B — countDocuments() with $text

- [ ] **Experiment 10** — Aggregation Pipeline with Text Search
  - [ ] Pipeline 1 — $match + $addFields score + $sort by relevance
  - [ ] Pipeline 2 — Paginated results ($skip + $limit)
  - [ ] Pipeline 3 — $count on text search results
  - [ ] Pipeline 4 — Exact phrase search inside pipeline
  - [ ] Pipeline 5 — Exclusion search inside pipeline
  - [ ] Pipeline 6 — $group to classify by relevance score
  - [ ] Pipeline 7 — Multi-word OR search with $split + $arrayElemAt
