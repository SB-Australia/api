# Automatic login for API partner app users

#### 0.) Prerequisites

 - You have to have a Squirrel Street API account
 - The user you are trying to log in must have been created by you through the Squirrel Street API

 
#### 1.) Create/use a client_credentials Oauth2 token issued by the Squirrel Street ID Service

```curl
curl -X POST \
-H "Content-Type:application/x-www-form-urlencoded" \
-u CLIENT_ID:CLIENT_SECRET \
https://id.squirrelstreet.com/oauth/token?grant_type=client_credentials&scope=TOKEN_SCOPE
```

And you get something like this back:

```json
{
    "access_token": "e8b11ffc-a725-43e9-885e-d787e6377951",
    "token_type": "bearer",
    "expires_in": 3600,
    "scope": "tokenscope"
}
```

**Note:**
> You will have to replace CLIENT_ID, CLIENT_SECRET and TOKEN_SCOPE
> with the appropriate values for your API client


#### 2.) Obtain an auth only Oauth2 access token for a certain user for the Squirrel Street ID Service

```curl
curl -X POST -H "Authorization: Bearer CLIENT_CREDENTIALS_TOKEN" \ https://id.squirrelstreet.com/user/applications/CLIENT_ID/users/USER_ID/tokens
```

And you get something like this back:

```json
{
    "token": "9d4778e9-55e2-40f2-8f7e-7d11abb4ed34",
    "validity": 3600
}
```
**Note:**
> You will have to replace the CLIENT_CREDENTIALS_TOKEN in the above call with
> the value obtained in step 1.)
>
> You will be able to use the auth only token to do automatic user login only.
>
> Also, you will be able to use it only once. Once used, it expires/vanishes.


#### 3.) Exchange the auth only Oauth2 token for a Squirrel Street user session

Somewhere on your site incorporate a link like this:

```html
<a href="https://id.squirrelstreet.com/gateway?key=TOKEN_VALUE"> Click to see your Squirrel Street documents </a>
```

When clicking on it, the user will get logged in automatically to the Squirrel Street application.

**Note:**
> You will have to replace TOKEN_VALUE with the value obtained at step 2.) 
>
> The oauth only token expires just as all the other access tokens. If you don't use it
> within the expiration timespan your link will not log the user automatically.
>
> You may want to do the above steps as a result of the user clicking on a generic
> Login To Squirrel Street link, obtain the auth only token on the fly and redirect to https://id.squirrelstreet.com/gateway?key=TOKEN_VALUE with the fresh obtained value.
