# MongoDB-Basics

## MongoDB Basic Course - 1 part -2 
  1. Advance search
  2. Schema validation
  3. Operators (Comparison Operators , Logical Operators , Combining Logical Operators , Element Operators , Array Operators)


### Advance search

#### single value 
```bash
db.students.find({ name: "tamil" }, { age: 1, _id: 0 })
```
#### find embedded value
```bash
db.students.find({ "address.postCode": 607302 })
```

#### find distinct value
```bash
db.students.distinct("address.postCode")
```

### Schema validation

#### create JSON Schema
```bash
db.createCollection("teachers", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "age", "email"],
      properties: {
        name: {
          bsonType: "string",
          description: "Name is required and must be a string."
        },
        age: {
          bsonType: "int",
          minimum: 18,
          description: "Age is required, must be an integer, and should be at least 18."
        },
        email: {
          bsonType: "string",
          pattern: "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$",
          description: "Email is required, must be a valid email address."
        }
      }
    }
  }
});
```

#### Updated JSON Schema

```bash
db.runCommand({
  collMod: "teachers",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "age", "email", "mobilenumber"],
      properties: {
        name: {
          bsonType: "string",
          description: "Name is required and must be a string."
        },
        age: {
          bsonType: "int",
          minimum: 18,
          description: "Age is required, must be an integer, and should be at least 18."
        },
        email: {
          bsonType: "string",
          pattern: "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$",
          description: "Email is required, must be a valid email address."
        },
        mobilenumber: {
          bsonType: "int",
          minimum: 1000000000,
          maximum: 9999999999,
          description: "Mobile number is required, must be a 10-digit integer."
        }
      }
    }
  },
  validationLevel: "strict"
});

```

### Operators
  #### Comparison Operators (eq,ne,gt,get,lt,let,in,nin)
  
   ```bash
  db.students.find({ age: { $eq: 18 } });
  ```
  ```bash
db.students.find({ age: { $ne: 18 } });
  ```
   ```bash
db.students.find({ marks: { $gt: 80 } });

  ```
 ```bash
db.students.find({ marks: { $gte: 85 } });

```
 ```bash
db.students.find({ age: { $lt: 20 } });
  ```
```bash
db.students.find({ marks: { $lte: 75 } });
  ```
```bash
 db.students.find({ marks: { $in: [72, 90] } });
  ```

```bash
db.students.find({ marks: { $nin: [72, 90] } });
```


  #### Logical Operators (and,or,nor,not )


```bash
db.students.find({ $and: [{age: 18 },{marks:{$gt:80}}] });
```
```bash
db.students.find({ $or: [ { age: 18 }, { marks: { $gt: 80 } } ] });
```
```bash
db.students.find({ $nor: [ { age: 18 }, { marks: { $gt: 80 } } ] });
```
```bash
db.students.find({ marks: { $not: { $gt: 80 } } });
```

#### Combining Logical Operators

```bash
db.students.find({ $or: [ { age: 18 }, { marks: { $gt: 85 } } ], $not: { marks: { $lt: 75 } } });
```
```bash
db.students.find({age: { $ne: 18 }, $or: [ { marks: { $gte: 72 } }, { marks: { $gt: 80 } } ] });
```

#### Element Operators (exists, type, JSON schema,expr)

```bash
db.students.find({ age: { $exists: true } });
```
```bash
db.students.find({ marks: { $type: "int" } });
```

```bash
db.students.find({ marks: { $jsonSchema: { bsonType: "int", minimum: 80 } } });
```

```bash
db.students.find({ $expr: { $gt: ["$highestMark", { $avg: "$marks" }] } });
```
```bash
db.students.find({ $expr: { $eq: ["$highestMark", { $max: "$marks" }] } });

```
```bash
db.students.find({ $expr: { $and: [{ $gt: ["$highestMark", 85] }, { $lt: ["$age", 19] }] } });

```
```bash
db.students.find({ $expr: { $or: [{ $gt: ["$highestMark", 90] }, { $eq: ["$age", 20] }] } });

```
```bash
db.students.find({ $expr: { $gt: [{ $arrayElemAt: ["$marks", 0] }, 70] } });

```
```bash
db.students.find({ $expr: { $not: { $eq: ["$highestMark", { $max: "$marks" }] } } });

```

#### Array Operators (elemMatch,all,size,push,addToSet,pop,pull,unwind)

```bash
db.students.find({ marks: { $elemMatch: { $gt: 80, $lt: 90 } } });

```
```bash
db.students.find({ marks: { $all: [75, 85] } });
```

```bash
db.students.find({ marks: { $size: 3 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $push: { marks: 95 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $addToSet: { marks: 95 } });
```

```bash
db.students.updateOne({ name: "Alice" }, { $pop: { marks: 1 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $pop: { marks: -1 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $pull: { marks: { $lt: 80 } } });
```

```bash
db.students.aggregate([ { $unwind: "$marks" } ]);
```
