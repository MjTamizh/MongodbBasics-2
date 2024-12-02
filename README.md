# MongoDB-Basics 2

## MongoDB Basic Course - 1 part -2 
  1. Advance search
  2. Schema validation 


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








