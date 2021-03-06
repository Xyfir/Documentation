Xyfir Accounts takes care of most of the problems encountered when creating an account system. We will take care of the registration, logins, security, forgotten passwords, and so on. You'll still have a small part in login and registration, but for the most part integrating Xyfir Accounts with your service requires very little work.

# Creating a Service

The first step in integrating Xyfir Accounts with your service is by adding your service to our database. You can start this process in the [Developer Dashboard](https://accounts.xyfir.com/dashboard/developer). Navigate to the _Create Service_ section, fill out the form, and once you've created your service we can begin adding Xyfir Accounts' functionality to your service. Be sure to grab your service's identifier number after you create your service. Your service's id is found next to your service's name in parentheses when viewing your service's info.

# Integrating Xyfir Accounts

There are three main things we must do to before users can login to your service using their Xyfir Account. Those three things are:

1.  Set a _Login_ link
2.  Set a _Register_ link
3.  Setup a system that can handle redirects and data passed from Xyfir Accounts after a login or registration

## Login/Register With Xyfir

These two steps are basically identical, and extremely easy to complete. Somewhere on your site, in your app, etc, you must place a link that'll take the user to Xyfir Accounts where they can then link your service to their account, or login to your service with their account. This is done by taking our login and registration links (`https://accounts.xyfir.com/login/service/` and `https://accounts.xyfir.com/register/service/`) and appending your service's identifier number given to you when viewing your service in the _Developer Dashboard_.

For example if your service's ID was `10`, your login link would be `https://accounts.xyfir.com/login/service/10`. Once you've added those links, your first two steps are done.

## Validating Logins (and Registrations)

After a user has logged into your service or linked your service to their account via Xyfir Accounts, we redirect their browser back to your site (the 'Login' API route) with two important variables in the url's query string: `auth` and `xid`.

### The Redirect

The link you provided when creating your service must be able to accept a `GET` request containing the following query string variables:

- `xid`: _string_ - The user's Xyfir ID which is a unique identifier for a user on a specific service. The Xyfir ID will be used for you to link a user to a set of data in your database. If the provided XID does not exist in your database, you create a new user. If the XID _does_ exist, you update the user's data.
  - `xid` can be up to a 64 character string that contains alphanumeric characters. Make sure your database is case-sensitive for this value to prevent matching a user on your service with the wrong xyfir id.
- `auth`: _string_ - The XID acts like a username while `auth` acts like a password. This prevents unauthorized logins in the event that a user's XID is compromised.

#### Example

If the 'Login' API route you set when creating your service was `https://mywebsite.com/login` we would send users to `https://mywebsite.com/login?xid=XID&auth=AUTH` after a user logs into your service through Xyfir Accounts or after they link your service to their account.

### Verifying the Login

Once you've received the `xid` and `auth` token of the user logging in, you must call Xyfir Accounts to verify the `xid`/`auth` and to retrieve the user's data.

### API Request

`GET https://accounts.xyfir.com/api/service/:service/user?key=KEY&xid=XID&token=TOKEN`

- `:service`: _number_ - Your service ID obtained when you created your service
- `KEY`: _string_ - Any one of your service's secret authentication keys, found by viewing your service in the service dashboard
- `XID`: _string_ - The user's Xyfir ID returned after login or registration
- `TOKEN`: _string_ - The user's authentication or access token

### API Response

```js
{
  // The user's email
  email: String,

  // Explains error if there was one
  message: String,

  // Explained in depth later in documentation
  // Can be safely ignored if service does not need access tokens
  accessToken: String
}
```

### Working with Response Data

Once you receive the data from the user, you must check if the `xid` already exists in your database to know if the user is already registered. If the user is not registered yet, add the user and the data returned from the previous API request to your database.

# Optional

## Logging Out

Once you've verified the login and created a new user or updated an existing user's data, you can then mark their session as logged in. After that there's only one last thing to deal with and then your login system is complete: logging out. Simply add a button or link somewhere that allows a user to logout of your service. This is done by wiping their session data or deleting their session entirely.

## Access Tokens

Access tokens allow your service to provide a more persistent type of session system while decreasing the need for your users to keep logging in to your service through Xyfir Accounts.

### Handling Access Tokens

An access token is returned as `accessToken` in the response from `GET https://accounts.xyfir.com/api/service/:service/user`. Your service must have some way of linking an access token to a user's Xyfir ID (xid). It is important to remember that a single user can have multiple access tokens for a single service, and also that while unlikely, multiple users can have the same access token.

The way Xyfir services like Ptorx or xyBooks handle access tokens is by encrypting a string that contains a user's internal user id (not their Xyfir ID) and the access token and sending this encrypted string to the client. This string is then stored by the client in local storage and then sent to the service's API upon the application loading to create a new session. This setup removes the need for the services to store access tokens on their servers.

### Using Access Tokens

`GET https://accounts.xyfir.com/api/service/:service/user`

Using access tokens is very similar to using a user's Xyfir ID and authentication token after a login from Xyfir Accounts. The access token takes the place of the authentication token while everything else remains the same. The response is the same as if you used an authentication token except that the API will not return an access token because the access token used in the request is still valid.

If the API returns a non-`200` status, you should clear the user's session if needed and then force the client to login to your service again by directing them to your Xyfir Accounts service login link.

If the API returns a `200` status, you should update your database to reflect the user data returned from the API as well as setting any needed session values.

## Listen For Account Changes

Xyfir Accounts also gives you the ability to be notified immediately when a user updates the data they provided your service or when they unlink your service from their account. This is done via the 'Update' and 'Unlink' API Routes.

xyAccounts will only send each request once regardless of your server's response.

### Unlink

xyAccounts will send a `DELETE` request to your 'Unlink' API route when the user unlinks your service from their account. The request will contain the following data in the body:

- `xid`: _string_ - The xid of the user who is unlinking your service.
- `key`: _string_ - Any one of your service's secret authentication keys.

# Notes

## Service Authentication Keys

Your service keys are used as an extra layer of security for requests between xyAccounts and your service. It is important that these keys stay secret and are never publicly exposed. You can delete and generate new keys at any time by viewing your service in the service dashboard.
