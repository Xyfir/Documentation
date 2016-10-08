# Xyfir Accounts
Xyfir Accounts takes care of most of the problems encountered when creating an account system. We will take care of the registration, logins, security, forgotten passwords, and much more. You'll still have a small part in login and registration, but for the most part integrating Xyfir Accounts with your service requires very little work.

Xyfir Accounts is also currently the only way to retrieve a user's Xyfir Ads profile ID. Meaning if you use Xyfir Ads on your service, integrating Xyfir Accounts will allow you to better personalize advertisements for your users.

## Creating a Service
The first step in integrating Xyfir Accounts with your service is by adding your service to our database. You can start this process in the [Service Dashboard](https://accounts.xyfir.com/service/dashboard). Navigate to the *Create Service* section, fill out the form, and once you've created your service we can begin adding Xyfir Accounts' functionality to your service. Be sure to grab your service's identifier number after you create your service. Your service's id is found next to your service's name in parentheses when viewing your service's info. *Service Name (**55**)*

## Integrating Xyfir Accounts
There are three main things we must do to before users can login to your service using their Xyfir Account. Those three things are:
1. Set a *Login with Xyfir* link
2. Set a *Register with Xyfir* link
3. Setup a system that can handle redirects and data passed from Xyfir Accounts after a login or registration

### Login/Register With Xyfir
These two steps are basically identical, and extremely easy to complete. Somewhere on your site, in your app, etc, you must place a link that'll take the user to Xyfir Accounts where they can then link your service to their account, or login to your service with their account. This is done by taking our login and registration links (`https://accounts.xyfir.com/app/#/login/` and `https://accounts.xyfir.com/app/#/register/`) and appending your service's identifier number given to you when viewing your service in the *Service Dashboard*.

For example if your service's ID was `55`, your login link would be `https://accounts.xyfir.com/app/#/login/55`. Once you've added those links, your first two steps are done.

### Validating Logins and Registrations
After a user has logged into your service or linked your service to their account via Xyfir Accounts, we redirect their browser back to your site with two important variables in the url's query string: `auth` and `xid`. The address we direct them to is the link you set when you created your service.

#### The Redirect
The link you provided when creating your service must be able to accept a `GET` request containing the following variables:
- `xid`: *string* - The user's Xyfir ID which is a unique identifier for a user on a specific service. The Xyfir ID will be used for you to link a user to a set of data in your database. If the provided XID does not exist in your database, you create a new user. If the XID *does* exist, you update the user's data.
    - `xid` is always a 64 character string that contains alphanumeric characters. Make sure your database is case-sensitive for this value to prevent matching a user on your service with the wrong xyfir id (and therefore the wrong data).
- `auth`: *string* - The XID acts like a username while `auth` acts like a password. This prevents unauthorized logins in the event that a user's XID is compromised.

##### Example
If the website link you set when creating your service was `https://mywebsite.com/login` we would send users to `https://mywebsite.com/login?xid=XID&auth=AUTH` after a user logs into your service through Xyfir Accounts or links your service to their account.

#### Verifying the Login
Once you've received the `xid` and `auth` token of the user logging in, you must call Xyfir Accounts to verify the `xid`/`auth` and to retrieve the user's data.

#### API Request
`GET https://accounts.xyfir.com/api/service/<SERVICE_ID>/<SERVICE_KEY>/<XID>/<AUTH_TOKEN>`
- `SERVICE_ID`: *number* - Your service ID obtained when you created your service
- `SERVICE_KEY`: *string* - Any one of your service's secret authentication keys, found by viewing your service in the service dashboard
- `XID`: *string* - The user's Xyfir ID returned after login or registration
- `AUTH_TOKEN`: *string* - The user's authentication token returned after login or registration

#### API Response
```js
{
    // True when service_id/service_key or xid/auth don't match
    error: boolean,

    // Explains error when error is true
    message: string,

    // The user's Xyfir Ads profile identifier
    xadid: string,

    // Explained in depth later in documentation
    // Can be safely ignored if service does not need access tokens
    accessToken: string,
    
    // Fields you marked as required or optional when creating your service
    // Fields marked required will be in this object
    // Fields marked optional may or may not be present
    fname: string, lname: string, address: string, zip: string,
    country: string, region: string, email: string,
    phone: string, gender: number,
    birthdate: string
}
```

#### Working with Response Data
Once you receive the data from the user, you must check if the `xid` already exists in your database (user is already registered). If the user is not registered yet, add the user and the data returned from the previous API request to your database. If the user was already registered, you **must** update the data in your database with the data the user provided. This is a requirement to being allowed to use our API.

## Optional

### Logging Out
Once you've verified the login and created a new user or updated an existing user's data, you can then mark their session as logged in. After that there's only one last thing to deal with and then your login system is complete: logging out. Simply add a button or link somewhere that allows a user to logout of your service. This is done by wiping their session data or deleting their session entirely.

### Access Tokens
Access tokens allow your service to provide a more persistent type of session system while decreasing the need for your users to keep logging in to your service through Xyfir Accounts.

#### Handling Access Tokens
An access token is returned as `accessToken` in the response from `GET https://accounts.xyfir.com/api/service/<SERVICE_ID>/<SERVICE_KEY>/<XID>/<AUTH_TOKEN>` when `error` is false. Your service must have some way of linking an access token to a user's Xyfir ID (xid). It is important to remember that a single user can have multiple access tokens for a single service, and also that while unlikely, multiple users can have the same access token.

The way Xyfir services like Ptorx or Vynote handle access tokens is by encrypting a string that contains a user's internal user id (not their Xyfir ID) and the access token and sending this encrypted string to the client. This string is then stored by the client in local storage and then sent to the service's API upon the application loading to create a new session. This setup removes the need for the services to store access tokens on their servers.

#### Using Access Tokens
`GET https://accounts.xyfir.com/api/service/<SERVICE_ID>/<SERVICE_KEY>/<XID>/<ACCESS_TOKEN>`

Using access tokens is very similar to using a user's Xyfir ID (`xid`) and authentication token (`auth`) after a login from Xyfir Accounts. The access token takes the place of the authentication token while everything else remains the same. The response is the same as if you used an authentication token except that the API will not return an access token because the access token used in the request is still valid.

If the API returns `error` as true, you should clear the user's session if needed and then force the client to login to your service again by directing them to your Xyfir Accounts service login link.

If the API returns `error` as false, you should update your database to reflect the user data returned from the API as well as setting any needed session values.

## Notes

### Service Authentication Keys
Your service keys are used as an extra layer of security when verifying a user's attempt to login to your service. It is important that these key stay secret and are never publicly exposed. You can delete and generate new keys at any time by viewing your service in the service dashboard.