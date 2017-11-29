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

When you test the above scenario, you'll notice that the rule denies access to the owner, if that owner is not listed as a friend of himself (which would be weird). So to allow the owner access to his own profile, we must adjust the rule as follows:

```
".read" : "data.child('friends').child(auth.uid).exists() || $uid == auth.uid"
```

The rule now reads:

> If the accessing user is a friend of the accessed user OR if it his own profile...


## Overwriting security rules on a lower level

We have now successfully implemented a good level of security while not restricting access too much. But we said earlier that we want to disclose the first and last name only, not the birthdate. More generally speaking, we want to divide the profile into a public and a private set of properties. The database for that could look as follows:

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

Structuring our database this way allows us to define rules for both part, `public` and `private` separately.

The rules for both parts could then look like this:

```javascript
{
  "rules": {
  
    "userprofiles": {

      "$uid": {
        ".write": "auth.uid == $uid",

        "public" {
            ".read": "data.child('public').child('friends').child(auth.uid).exists() || auth.id = $uid",
        },
        
        "private": {
            ".read": "auth.uid == $uid"
        }
      }

    }
  }
}
```

The public part is accessible for all friends and the user himself, while the private part is accessible only to the user himself.

## Listing profiles with read permission

Note that, with rule file above, you cannot access the `userprofiles` node and iterate through all profiles in JavaScript. For example, this would lead to an error, because there is no `.read` rule on the `userprofiles` node that would allow you to read the whole node:

```javascript
firebase.database().ref('userprofiles').once( ... );
```

Instead, if you want to list users that the currently signed in user has access to, you could maintain another list in your database where you only store the `uid`s of all users. This list is readable for all signed in users. You can then read the `uid`s from the list and make individual requests for each user profile. The database would then look like this:

```javascript
"uids": {
  "6842c1pSL8OP34Bf812JRq6e7Yh1": true,
  ...
}

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

We can then use the following code to iterate through the public and private parts of each profile and see if we have access:

```javascript
firebase.database().ref('uids').once(function(uids) {

  var uids = Object.keys(uids.val());
  uids.forEach(uid => {
  
    firebase.database().ref('userprofiles/' + uid + "/public").once(publicProfile => {
      console.log(publicProfile);
    }, handleAccessDeniedError);
    
    firebase.database().ref('userprofiles/' + uid + "/private").once(privateProfile => {
      console.log(privateProfile);
    }, handleAccessDeniedError);
  });
});

handleAccessDeniedError(error) {
  console.error(error.message);
}
```

That's it! Try and implement rules to meet your security requirements!