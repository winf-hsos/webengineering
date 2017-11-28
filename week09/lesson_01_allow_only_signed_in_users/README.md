# How to: Only allow signed in users to read or write data

In previous weeks, we have learned how we can register and sign in users. This is useful for many reasons, for example to store information about the user in a user profile.

Until now, our database was open for anyone to read and write. This served us well when we played around with reading and writing, as we didn't have to deal with authentication and authorization. Now that we want to store sensitive information related to our users, it is time that we deal with securing our database.

## Introduction to Firebase rules

The way to secure the database is through the definition of security rules. We have already introduced one rule when we started working with the Firebase real time database:

```javascript
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

Remember you can find the current rules when you navigate to real time database in the Firebase console and then click on the rules tab.

![Where to find the Firebase rules](/media/firebase-database-rules-where-to-find-them.gif)