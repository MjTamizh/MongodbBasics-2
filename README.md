# MongoDB-Basics 2

## MongoDB Basic Course - 1 part -2 
  1. Advance search
  2.  


### Advance search

#### single value 
```bash
db.students.find({ name: "tamil" }, { age: 1, _id: 0 })
```
#### find embedded value
```bash
db.students.find({ "address.postCode": 607302 })
```
