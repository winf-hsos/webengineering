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

Remember, you can find the current rules when you navigate to real time database in the Firebase console and then click on the rules tab.

![Where to find the Firebase rules](/media/firebase-database-rules-where-to-find-them.gif)

The rule above introduces two important rule types we can define for any data: `.read` and `.write`. In the example above, we state that both properties are `true`, which implies that anyone can read from and write to our database. This is very unsecure, this exposes all our data to the world. Let's improve upon that.

## Restricting access to signed-in users

In the first step, let's allow only signed-in users to read and write to our database. This is major step of improvement, as we can control who we grant an account and therefore trust with our web app.

Finding out if a user is signed in is straightforward. Firebase gives us access to the currently authentication state via the `auth` variable. If this variable is not null, expressed as `auth != null`, we know that someone is signed in:

```javascript
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

The above set of rules implies that only signed-in users can read from and write to our database. These rules apply to all data in our database, because we defined them on the topmost level. We can control which data is affected by a rule by assigning the rule to the right node in our JSON tree.

## Controling access for specific data nodes

Say we have the following database structure with two nodes:

```javascript
"root" : {
    "products" {
        ...
    }
    
    "userprofiles" : {
        ...
    }
}
```

In the `products` node, we store products to sell via our webshop. These products and their information should be visible regardless if the user is signed in or not. We don't want anyone to change the products, though. So we forbid any write access.

In the `userprofiles` node, we store sensitive user data, and we can only allow signed-in users to read here. Anyone coming to our website anonymously should never see data from a user profile.

This means we need two different rules: One for the `products` node to allow access to anyone, and another one for the `userprofiles` node to restrict access to signed-in users. We have seen both rules we need above, now we need to assign them to the right nodes:

```javascript
{
  "rules": {

    "products": {
        ".read": true,
        ".write": false
    }

    "userprofiles": {
        ".read": "auth != null",
        ".write": "auth != null"
    }        
  }
}
```

As you can see, assigning a rule to a specific node in the JSON tree is as simple as reproducing the structure of our database and putting the rules in the right place. That is, the right node.

Watch the video below for a great introduction to Firebase security rules (don't worry about the title, no need to know SQL).

[![Securing your data structure with Security Rules](https://img.youtube.com/vi/rtoxRg-kbt0/0.jpg)](https://www.youtube.com/watch?v=rtoxRg-kbt0)

## Deploying database rules

There are two ways to define database rules: 

1. You can write your rules directly in the Firebase console
2. You can define your rules in your `database.rules.json` file in your workspace. Anytime you deploy with `firebase deploy`, the content of your rules file is deployed to Firebase.

So whenever you define rules directly in the Firebase console, make sure you update your `database.rules.json` file accordingly. Otherwise your rules will be overwritten with the next deployment of your code.

A good process ist to test new rules directly in the Firebase console. The simulator cann help you to test your new rules. When the rules works as expected, update your `database.rules.json` file to contain the new rule.

![The Firebase Rules Simulator](/media/firebase-rules-simulator.gif)