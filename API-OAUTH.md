# OAUTH 2.0 authentication for API

In order to access Deep Intelligence API you need an authorization token. You can obtain one manually from your [Account settings](https://app.deepint.net/o/none?ws=&s=account) or you can create one for an external user using OAuth 2.0.

The first step is to create a new application. You can do it in your [Account settings](https://app.deepint.net/workspace?ws=&s=account).  You must set:
 - The application name.
 - The callback URL. When the user authorizes your application or denies it, they will be redirected to the provided URL.
 - The permission level you want to request to the users.

Once created, you will be provided with:
 - The client ID
 - The client secret (keep it safe, if you compromise it, you should remove your application and create a new one).
 - The authorization URL.

In order to request access to an account, redirect the user to the authorization URL. 

If the user authorizes your application, they will be redirected to the callback URL with a parameter `bearer` in the URL, corresponding to the bearer token, a single-use token in order to create a new authorization token.

If the user denies the access, they will be redirected to the callback URL, without any extra parameters.

With the bearer token, send a *POST* request to `https://app.deepint.net/api/oauth/login` with the following parameters:
- `client_id` - The client ID of your application
- `client_secret` - The client secret of your application.
- `bearer` - The received bearer token

The request will end with STATUS = 200 if the bearer is valid, and you will be provided in the response body with a JSON containing the authentication token:

```json
{ "result": "success", "auth_token": "XXXXXXXXXXXXXXXXXXXXXXXXX" }
```

With the authentication token now you can access the API:

 - [https://app.deepint.net/api/v1/documentation/](https://app.deepint.net/api/v1/documentation/)
