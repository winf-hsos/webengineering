# How to: Allow only the owner and friends to read a profile

Allowing only the owner of a profile to read and write is a big improvement in terms of security. But what if we want to allow certain other users to view the profile as well, and if only a subset of the profile? The following describes how to solve this with Firebase database rules.

## Lookups with database rules

Consider we have the following structure of user profiles in our database:

```javascript
"userprofiles": {

    "6842c1pSL8OP34Bf812JRq6e7Yh1": {
        "firstname": "Max",
        "lastname": "Mustermann",
        "dateofbirth": "01.01.1990",
        "friends" : {
            "DMpMS4IY5wauWoV4gOq6vC8lp4t2": true
        }
    },
    
    "DMpMS4IY5wauWoV4gOq6vC8lp4t2": {
        "firstname": "Erika",
        "lastname": "Mustermann",
        "dateofbirth": "24.12.1987",
        "friends" : {
            "6842c1pSL8OP34Bf812JRq6e7Yh1" : true
        }
    }
}

```

This is the same example as from [lesson 2](../lesson_02_allow_only_owner/userprofiles.json), but we added the property `friends`. This new property contains a list of other users which are friends of this user. In our scenario, we want to allow all of our friends to be able to see our firstname and lastname, but not our birthdate.

To achieve this, we need to check wether the `uid` of the user trying to access a profile is contained on the friends list of the profile. This is referred to as a *lookup* and can be done with Firebase as follows:

```
".read" : "data.child('friends').child(auth.uid).exists()"
```

The `data` server variable gives us access to the current node that is being accessed. Therefore, the `data` variable contains a user's profile. We then access a child node of the profile named `friend`, which is our list of friends we previously introduced. Then, we try to get a child of this list with the name `auth.uid`, which resolved to the `uid` of the currently signed-in user. If this child exists, then the currently signed-in user is a friend of the user whos profile is being accessed. Therefore, the read permission is granted.

We can now test if our rules works using the simulator in the Firebase console.

![The Firebase rules simulator](/media/firebase-rules-simulator.gif)

If the rules work (and they should), we must update our `database.rules.json` file accordingly:

```javascript
{
  "rules": {
    "products": {
      ".read": true,
      ".write": false
    }
    
    "userprofiles": {

      "$uid": {
        ".read": "data.child('friends').child(auth.uid).exists()",
        ".write": "auth.uid == $uid"
      }
    }
  }
}
```

We have now successfully implemented a good level of security while not restricting access too much. But we said earlier that we want to disclose the first and last name only, not the birthdate. We need one last tweak:


```javascript
{
  "rules": {
    "products": {
      ".read": true,
      ".write": false
    }
    
    "userprofiles": {

      "$uid": {
        ".read": "data.child('friends').child(auth.uid).exists()",
        ".write": "auth.uid == $uid",
        
        "dateofbirth": {
            ".read": "auth.uid == $uid"
        }
      }
    }
  }
}
```

We added a new rule for the child node `dateofbirth` of a user's profile, which states that only the owner may read this property. You can see an important concept of Firebase rules in action here: Rules on a lower level (more specific) overwrite rules on a higher level in the hierarchy. This works great if we only have one or two properties that we want to hide from friends.

If we have more properties, defining a special rule for every field can lead to unreadable and complex rules. In this case, it would be better to split the user profile in two parts, one called `public` and one called `private`, for example. We can then define the access rules for these two nodes only:

```javascript
"userprofiles": {

    "6842c1pSL8OP34Bf812JRq6e7Yh1": {
    
      "public": {
        "firstname": "Max",
        "lastname": "Mustermann",
        "friends" : { ... }
       },
       
      "private": {
          "dateofbirth": "...",
          "hobbies:": "..."
      }
    }
}
```

The rules could then look like this:

```javascript
{
  "rules": {
  
    "userprofiles": {

      "$uid": {
        
        ".write": "auth.uid == $uid",
      
        "public" {
            ".read": "data.child('public').child('friends').child(auth.uid).exists()",
        },
        
        "private": {
            ".read": "auth.uid == $uid"
        }
      }

    }
  }
}
```

That's it! Try and implement rules to meet your security requirements!