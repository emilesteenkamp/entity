# Entity

## What is Entity

Entity is a Kotlin MongoDB ORM.

## How does it work

To define an Entity:

```kotlin
package entity

import CollectionName
import DatabaseName
import Entity
import org.bson.types.ObjectId
import kotlin.collections.ArrayList

@DatabaseName("user")
@CollectionName("user")
class User(
    override val _id: ObjectId?,
    val name: UserName,
    val age: Number,
    var status: UserStatus = UserStatus.ACTIVE,
    val countriesVisited: ArrayList<String> = arrayListOf()
) : Entity(_id)

enum class UserStatus {
    ACTIVE,
    INACTIVE
}

class UserName(val firstName: String, val lastName: String) {}
```

After defining an Entity, you can easily create and save an entity using:
```kotlin
val user: User = User(
    null,
    UserName("Emile", "Steenkamp"),
    23,
    UserStatus.ACTIVE,
    arrayListOf("ZA", "NL")
).save()
```

The above defined `User` object will be saved in the database as:
```json
{
  "_id": ObjectId("6153263f8aedab15aa1f44d4"),
  "age": 23,
  "countriesVisited": [ "ZA", "NL" ],
  "name": {
    "firstName": "Emile",
    "lastName": "Steenkamp"
  },
  "status": 0
}
```

After saving the object, we can find an Entity by its `_id`:
```kotlin
val user: User = Entity.findById(ObjectId("6153263f8aedab15aa1f44d4"))
```

Update an Entity:
```kotlin
user.status = UserStatus.INACTIVE
user.save()
```