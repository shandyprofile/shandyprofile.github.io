---
title: 'JWT Authentication'
description: ""
author: [shandy]
date: 2025-10-12
updateDate: 
categories: [(ExpressJS) Server-Side development, Express JS]
tags: [Express JS]
sort_index: 110
# pin: true
# media_subpath: '/posts/02'
---

## 1. BASIC CRUD (Create, Read, Update, Delete)

| Category      | Command                                        | Description                                    |
| ------------- | ---------------------------------------------- | ---------------------------------------------- |
| **Create** | `Model.create(data)`                           | Insert a new document                          |
|               | `new Model(data).save()`                       | Create and save manually                       |
| **Read**   | `Model.find()`                                 | Get all documents                              |
|               | `Model.findOne(filter)`                        | Get one document that matches filter           |
|               | `Model.findById(id)`                           | Get document by its `_id`                      |
|               | `Model.find(filter, projection)`               | Filter and select specific fields              |
| **Update** | `Model.updateOne(filter, update)`              | Update the first matching document             |
|               | `Model.updateMany(filter, update)`             | Update multiple documents                      |
|               | `Model.findByIdAndUpdate(id, update, options)` | Update by ID and optionally return updated doc |
| **Delete** | `Model.deleteOne(filter)`                      | Delete first matching document                 |
|               | `Model.deleteMany(filter)`                     | Delete multiple documents                      |
|               | `Model.findByIdAndDelete(id)`                  | Delete by ID                                   |

### Examples

```json
// User
[
  {
    "_id": "652a12c4d12e1a2b5f8f3d9a",
    "name": "Alice",
    "email": "alice@example.com",
    "age": 25,
    "status": "active",
    "role": "user",
    "createdAt": "2025-09-15T10:30:00Z"
  },
  {
    "_id": "652a12c4d12e1a2b5f8f3d9b",
    "name": "Bob",
    "email": "bob@example.com",
    "age": 30,
    "status": "inactive",
    "role": "admin",
    "createdAt": "2025-08-12T09:45:00Z"
  },
  {
    "_id": "652a12c4d12e1a2b5f8f3d9c",
    "name": "Charlie",
    "email": "charlie@example.com",
    "age": 35,
    "status": "active",
    "role": "manager",
    "createdAt": "2025-07-22T14:15:00Z"
  },
  {
    "_id": "652a12c4d12e1a2b5f8f3d9d",
    "name": "Diana",
    "email": "diana@example.com",
    "age": 28,
    "status": "inactive",
    "role": "user",
    "createdAt": "2025-06-01T12:00:00Z"
  },
  {
    "_id": "652a12c4d12e1a2b5f8f3d9e",
    "name": "Ethan",
    "email": "ethan@example.com",
    "age": 40,
    "status": "active",
    "role": "superadmin",
    "createdAt": "2025-05-18T16:45:00Z"
  }
]
```

1. CREATE

```js
// Example 1: Simple create
await User.create({ name: "Alice", age: 25 });

// Example 2: Using constructor + save
const user = new User({ name: "Bob", age: 30 });
await user.save();
```

> The first method inserts directly; the second creates an instance, allowing you to run pre-save hooks or validations before saving.

2. READ

```js
// Example 1: Get all users
const users = await User.find();

// Example 2: Find by filter
const activeUsers = await User.find({ status: "active" });

// Example 3: Find one specific user
const user = await User.findOne({ email: "test@gmail.com" });

// Example 4: Find by ID
const user = await User.findById("652a12c4d12e1a2b5f8f3d9c");

// Example 5: Filter and select fields
const names = await User.find({}, "name email");
```

> - `find()` returns an array; `findOne()` and `findById()` return a single document. 
> - You can use projections (e.g., "name email") to limit returned fields.

3. UPDATE

```js
// Example 1: Update one
await User.updateOne({ name: "Alice" }, { $set: { age: 26 } });

// Example 2: Update many
await User.updateMany({ status: "inactive" }, { $set: { status: "active" } });

// Example 3: Update by ID and return the new version
const updatedUser = await User.findByIdAndUpdate(
  "652a12c4d12e1a2b5f8f3d9c",
  { $set: { age: 35 } },
  { new: true }
);
```

> - `updateOne` and `updateMany` modify existing documents.
> - `findByIdAndUpdate` lets you specify { new: true } to return the updated document instead of the old one.

4. DELETE

```js
// Example 1: Delete one
await User.deleteOne({ name: "Alice" });

// Example 2: Delete many
await User.deleteMany({ status: "inactive" });

// Example 3: Delete by ID
await User.findByIdAndDelete("652a12c4d12e1a2b5f8f3d9c");
```

> - `deleteOne` and `deleteMany` remove data based on filters.
> - `findByIdAndDelete` removes a document directly by _id.

## 2. COMMON QUERY PATTERNS

| Purpose             | Example                                         | Description                  |
| ------------------- | ----------------------------------------------- | ---------------------------- |
| Get all             | `Model.find({})`                                | Fetch everything             |
| Filter              | `Model.find({ age: 20 })`                       | Match certain fields         |
| Comparison          | `{ age: { $gt: 18 } }`, `{ age: { $lte: 30 } }` | `$gt`, `$lt`, `$gte`, `$lte` |
| Multiple conditions | `{ $and: [{ age: 20 }, { active: true }] }`     | Combine conditions           |
| Value in list       | `{ age: { $in: [18, 20, 25] } }`                | `$in`, `$nin`                |
| Fuzzy search        | `{ name: { $regex: 'john', $options: 'i' } }`   | Case-insensitive search      |
| Check existence     | `{ email: { $exists: true } }`                  | Field existence              |
| Null check          | `{ field: null }`                               | Match null fields            |

### Examples:

1. Get all

Fetch every document in the users collection.

```js
const users = await User.find({});
```

> Returns all documents in the collection, equivalent to SELECT * FROM users in SQL.

2. Filter

Find users who are exactly 20 years old.

```js
const youngUsers = await User.find({ age: 20 });
```

> Matches documents with age = 20.

3. Comparison Operators

Find users older than 18 and younger or equal to 30.

```js
const result = await User.find({ age: { $gt: 18, $lte: 30 } });
```

> $gt = greater than, $lte = less than or equal.

4. Multiple Conditions

Find users who are 20 years old and active.

```js
const users = await User.find({ $and: [{ age: 20 }, { status: 'active' }] });
```

> $and combines multiple filters; $or works similarly for alternatives.

5. Value in List

Find users whose age is either 18, 20, or 25.

```js
const users = await User.find({ age: { $in: [18, 20, 25] } });
```

> $in checks for membership; $nin is the opposite (not in list).

6. Regex (Regular Expression)

Find users whose name contains "john", case-insensitive.

```js
const users = await User.find({ name: { $regex: 'john', $options: 'i' } });
```

> $regex supports pattern matching like SQL LIKE '%john%'.

7. Check Existence

Find users who have an email field defined.

```js
const users = await User.find({ email: { $exists: true } });
```

> $exists returns documents whether a field is present or not.

8. Null Check

Find users where the field nickname is null.

```js
const users = await User.find({ nickname: null });
```

> Matches documents where nickname is null or not defined.

 
## 3. MONGODB OPERATORS

| Type           | Operators                                               | Meaning                               |
| -------------- | ------------------------------------------------------- | ------------------------------------- |
| **Comparison** | `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`              | Equals, not equals, greater/less than |
| **Logical**    | `$and`, `$or`, `$nor`, `$not`                           | Combine logic conditions              |
| **Array**      | `$in`, `$nin`, `$all`, `$size`                          | Match array values                    |
| **Update**     | `$set`, `$inc`, `$push`, `$pull`, `$addToSet`, `$unset` | Modify data                           |
| **Check**      | `$exists`, `$type`                                      | Check if field exists or its type     |

1. Comparison

Used in queries to filter by specific values or ranges.

```js
// Equal to 25
await User.find({ age: { $eq: 25 } });

// Not equal to 'inactive'
await User.find({ status: { $ne: 'inactive' } });

// Greater than or equal to 30
await User.find({ age: { $gte: 30 } });

// Less than 40
await User.find({ age: { $lt: 40 } });
```

> These operators are used for direct value comparisons — equivalent to SQL’s **=, !=, >, <, >=, <=**.

2. Logical

Used to combine or negate multiple filter conditions.

```js
// Match users who are active AND older than 25
await User.find({ $and: [{ status: 'active' }, { age: { $gt: 25 } }] });

// Match users who are either admin OR manager
await User.find({ $or: [{ role: 'admin' }, { role: 'manager' }] });

// Match users who are NOT active
await User.find({ status: { $not: { $eq: 'active' } } });
```

> **$and, $or, $not, $nor** help you build complex query logic.

3. Array Operators

Used when working with arrays inside documents.

```js
// Match if 'skills' contains at least one of these values
await User.find({ skills: { $in: ['NodeJS', 'React'] } });

// Match if 'skills' includes ALL listed values
await User.find({ skills: { $all: ['MongoDB', 'Express'] } });

// Match arrays with exactly 3 items
await User.find({ skills: { $size: 3 } });
```

> $in and $nin check membership; $all checks that all specified values are present; $size checks array length.

4. Update Operators

Used in updateOne(), updateMany(), or findByIdAndUpdate() to modify data.

```js
// Set a new field value
await User.updateOne({ name: 'Alice' }, { $set: { status: 'active' } });

// Increment a number field
await User.updateOne({ name: 'Bob' }, { $inc: { age: 1 } });

// Add an item to an array
await User.updateOne({ name: 'Charlie' }, { $push: { skills: 'Docker' } });

// Remove an item from an array
await User.updateOne({ name: 'Charlie' }, { $pull: { skills: 'React' } });

// Add only if not already in array
await User.updateOne({ name: 'Ethan' }, { $addToSet: { skills: 'AI' } });

// Remove a field completely
await User.updateOne({ name: 'Diana' }, { $unset: { nickname: "" } });
```

> hese are essential for updating documents without overwriting the entire object.

5. Check Operators

Used to inspect whether a field exists or to verify its type.

```js
// Find documents that have an 'email' field
await User.find({ email: { $exists: true } });

// Find where 'age' is stored as a Number
await User.find({ age: { $type: 'number' } });
```

> - $exists returns documents with or without a given field.
> - $type checks BSON type (e.g., number, string, date, objectId).

## 4. LIMIT, SORT, PAGINATION

| Feature        | Example                       | Description                    |
| -------------- | ----------------------------- | ------------------------------ |
| Limit          | `.limit(10)`                  | Limit number of results        |
| Skip           | `.skip(20)`                   | Skip first N results           |
| Sort           | `.sort({ age: 1 })`           | 1 = ascending, -1 = descending |
| Select fields  | `.select('name age')`         | Include only certain fields    |
| Exclude fields | `.select('-password -email')` | Exclude specific fields        |

### Examples

1. Limit results

Retrieve only the first 10 users from the database.

```js
const users = await User.find().limit(10);
```

> Used for efficiency — prevents returning too many records at once.

2. Skip results

Skip the first 20 users and return the rest.

```js
const users = await User.find().skip(20);
```

> Commonly used in pagination systems to “jump” to a later set of results.

3. Sort results

Sort users by age in ascending or descending order.

```js
// Ascending (youngest first)
const ascUsers = await User.find().sort({ age: 1 });

// Descending (oldest first)
const descUsers = await User.find().sort({ age: -1 });
```

You can also sort by multiple fields:

```js
await User.find().sort({ status: 1, age: -1 });
```

4. Skip results

Retrieve only specific fields like name and age.

```js
const users = await User.find().select('name age');
```

> - Equivalent to SQL SELECT name, age FROM users;
> - Reduces data load by excluding unnecessary fields.

5. Exclude fields

Hide sensitive or unnecessary data such as passwords or emails.

```js
const users = await User.find().select('-password -email');
```

> The minus sign (-) means “exclude this field” — useful for security.

6. Combine for pagination

Typical pagination query example:

```js
const page = 2;
const limit = 5;
const skip = (page - 1) * limit;

const users = await User.find()
  .sort({ createdAt: -1 })
  .skip(skip)
  .limit(limit);
```

> This retrieves page 2, with 5 users per page, sorted by newest creation date.


## 5. POPULATE (JOIN BETWEEN COLLECTIONS)

```js
const posts = await Post.find().populate('author', 'name email');
```

| Command                                                  | Description                  |
| -------------------------------------------------------- | ---------------------------- |
| `.populate('refField')`                                  | Fetch referenced document    |
| `.populate('refField', 'name age')`                      | Fetch only selected fields   |
| `.populate({ path: 'author', match: { active: true } })` | Apply filter inside populate |

### Examples:

JSON Plus for Posts collections:

```json
[
  {
    "_id": "700a12c4d12e1a2b5f8f3aaa",
    "title": "AI in Business",
    "content": "Using AI to optimize workflows.",
    "author": "652a12c4d12e1a2b5f8f3d9a",
    "likes": 45,
    "tags": ["AI", "Business"],
    "createdAt": "2025-09-16T09:00:00Z"
  },
  {
    "_id": "700a12c4d12e1a2b5f8f3aab",
    "title": "Managing Teams Effectively",
    "content": "Leadership strategies for modern managers.",
    "author": "652a12c4d12e1a2b5f8f3d9c",
    "likes": 23,
    "tags": ["Leadership", "HR"],
    "createdAt": "2025-09-20T14:30:00Z"
  },
  {
    "_id": "700a12c4d12e1a2b5f8f3aac",
    "title": "System Architecture",
    "content": "Designing scalable NodeJS backends.",
    "author": "652a12c4d12e1a2b5f8f3d9b",
    "likes": 58,
    "tags": ["Tech", "Backend"],
    "createdAt": "2025-09-25T10:00:00Z"
  }
]
```

1. Basic populate (simple JOIN)

Fetch all posts and include full author details.

```js
const posts = await Post.find().populate('author');
```

Output:

```
[
  {
    "title": "AI in Business",
    "author": { "name": "Alice", "email": "alice@example.com", "role": "user" }
  },
  {
    "title": "System Architecture",
    "author": { "name": "Bob", "email": "bob@example.com", "role": "admin" }
  }
]
```

2. Populate only specific fields

Fetch only the author’s name and email.

``` js
const posts = await Post.find().populate('author', 'name email');
```

Output:

```
[
  {
    "title": "AI in Business",
    "author": { "name": "Alice", "email": "alice@example.com" }
  }
]
```

3. Populate with filter (match condition)

Only populate authors whose status is "active".

```js
const posts = await Post.find().populate({
  path: 'author',
  match: { status: 'active' },
  select: 'name email status'
});
```

Output:
```
[
  {
    "title": "AI in Business",
    "author": { "name": "Alice", "email": "alice@example.com", "status": "active" }
  },
  {
    "title": "System Architecture",
    "author": null // Bob was filtered out (inactive)
  }
]
```

4. Nested populate (populate inside populate)

Assume there’s another collection Comment that references both Post and User:

```
const Comment = mongoose.model('Comment', new mongoose.Schema({
  post: { type: mongoose.Schema.Types.ObjectId, ref: 'Post' },
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  content: String,
  createdAt: Date
}));
```

Now fetch all comments, including both:
- The user who wrote the comment, and
- The post details, including its author.

```
const comments = await Comment.find()
  .populate({
    path: 'post',
    populate: { path: 'author', select: 'name email' }
  })
  .populate('user', 'name');
```

Output:

```
[
  {
    "content": "Great article!",
    "user": { "name": "Diana" },
    "post": {
      "title": "AI in Business",
      "author": { "name": "Alice", "email": "alice@example.com" }
    }
  }
]
```

5. Populate multiple references at once

If your Post schema had both author and reviewer, you can populate both in one query:

```
const posts = await Post.find()
  .populate('author', 'name')
  .populate('reviewer', 'name');
```

## 6. AGGREGATION PIPELINE

| Stage      | Example                                              | Description      |
| ---------- | ---------------------------------------------------- | ---------------- |
| `$match`   | `{ $match: { active: true } }`                       | Filter documents |
| `$group`   | `{ $group: { _id: '$status', count: { $sum: 1 } } }` | Group and count  |
| `$sort`    | `{ $sort: { age: -1 } }`                             | Sort results     |
| `$project` | `{ $project: { name: 1, age: 1 } }`                  | Select fields    |
| `$lookup`  | Lookup another collection (JOIN)                     |                  |
| `$unwind`  | Deconstruct an array field                           |                  |

Example:

```js
const result = await User.aggregate([
  { $match: { status: 'active' } },
  { $lookup: {
      from: 'posts',
      localField: '_id',
      foreignField: 'author',
      as: 'posts'
  }},
  { $unwind: '$posts' },
  { $group: {
      _id: '$name',
      totalPosts: { $sum: 1 },
      avgLikes: { $avg: '$posts.likes' }
  }},
  { $sort: { totalPosts: -1 } }
]);
```

> - $match: only active users
> - $lookup: join user with their posts
> - $unwind: flatten the array of posts
> - $group: group by user and count/average
> - $sort: show users with most posts first

## 7. COUNT & EXISTENCE CHECKS

| Command                    | Description                           |
| -------------------------- | ------------------------------------- |
| `countDocuments(filter)`   | Count documents that match filter     |
| `estimatedDocumentCount()` | Fast count of all documents           |
| `exists(filter)`           | Returns true/false if document exists |

Examples:

```js
// Count all active users
const activeCount = await User.countDocuments({ status: 'active' });

// Get total number of users quickly
const totalCount = await User.estimatedDocumentCount();

// Check if a user with a given email exists
const isExisting = await User.exists({ email: 'alice@example.com' });

console.log({ activeCount, totalCount, isExisting });
```

## 8. COMBINED ADVANCED QUERY EXAMPLE

Get a list of active users along with their posts, show only selected fields, compute total posts and average likes per user, then sort by total posts (descending).

```js
const result = await User.aggregate([
  // 1. Filter only active users
  { 
    $match: { status: 'active' } 
  },

  // 2. Join with Post collection (JOIN equivalent)
  { 
    $lookup: {
      from: 'posts',
      localField: '_id',
      foreignField: 'author',
      as: 'posts'
    } 
  },

  // 3. Add computed fields (totalPosts, avgLikes)
  { 
    $addFields: {
      totalPosts: { $size: '$posts' },
      avgLikes: { $avg: '$posts.likes' }
    } 
  },

  // 4. Project only desired fields
  { 
    $project: { 
      name: 1, 
      email: 1, 
      totalPosts: 1, 
      avgLikes: { $round: ['$avgLikes', 1] } 
    } 
  },
  // 5. Sort descending
  { 
    $sort: { totalPosts: -1 } 
  }
]);
```