## Login with Facebook

The example extends the standard login form using email and password with a new button to sign in with your Facebook account. For this to work, you'll need to adjust two settings in your [firebase console](https://console.firebase.google.com/):

1. Enable Facebook Authentication (Authentication -> Methods -> Provider)
2. Add your Cloud9 domain (e.g. `<workspace>-<username>.c9users.io`) to the list of allowed domains (Authentication -> Methods -> Authorized Domains)
3. Get a new APP ID and your App-Secret from the [Facebook Developers Console](https://developers.facebook.com)
4. In your Facebook app, add the "Facebook Login" product
5. Add the OAuth redirect domain to the list of allowed domains

