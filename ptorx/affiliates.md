Ptorx's affiliate system allows you to generate pre-verified Xyfir accounts that are linked to Ptorx accounts and have a Ptorx Premium subscription.

# Discounts

By default, affiliates are given a $0.99 (USD) discount for all accounts they create using the API. As you pay off previously created accounts, your discount will increase.

For every **one** account you create and pay off, your discount will increase by $0.01. If you've paid off less than 99 accounts, then your discount will be the minimum $0.99. If you've paid off 250 or more accounts, your discount will stop increasing after $2.49.

# Paying Off Created Accounts

Using the [Affiliate Panel](https://ptorx.com/affiliate), you can pay off your created accounts any time you wish. You have **31** days to pay off your owed accounts. Once **32** days of no payment has been hit, any unpaid accounts that are at least 32 days old will be deleted from Ptorx.

It is recommended that you pay off your created accounts once or twice a month to be safe and not risk losing any accounts.

# API Usage

First and foremost, you'll need your affiliate id, and your affiliate API key. These can be found in the [Affiliate Panel](https://ptorx.com/affiliate).

To authenticate, use basic HTTP authentication where the username is your affiliate id, and the password is your API key. Example: `https://123:8556b49c-77ca-4b03-87d8-a185d2a0c7d5@ptorx.com/api/affiliates`, where `123` is your ID, and `8556b49c-77ca-4b03-87d8-a185d2a0c7d5` is your key. You must provide the id and key with every request.

Responses will always be in JSON. Request bodies should use JSON or form url encoding.

Successful requests will return a status `200`, and unsuccessful requests will likely return a status `400`.

## Creating Accounts

Create an account on Xyfir Accounts, link that account to Ptorx, and give the Ptorx account a one-year subscription.

`POST` to `https://ptorx.com/api/affiliates/accounts`

## Body

* `email`: `string` _required_ - The email to create an account for. Cannot already have a verified Xyfir account. Must be an actual address that can receive mail, otherwise the user will not be able to login.
* `note`: `string` _optional_ - A note describing the account.

## Response

```json
{
  "message": "Some error message",
  "user_id": 123
}
```

`message` is only present if there was an error.

`user_id` is only present if a user was created. This user id can be used in later requests. You can ignore it if you wish.

## Deleting Accounts

Delete an account on Ptorx, but not on Xyfir Accounts or any other services linked to their Xyfir account. All linked proxy emails will be deleted and will no longer accept mail.

Note that deleting an account _before_ it gets paid off means you will not be charged for it.

`DELETE` to `https://ptorx.com/api/affiliates/accounts/:userId`

`:userId` is `user_id` as returned from the account creation API.

## Response

```json
{
  "message": "Some error message"
}
```

`message` is only present if there was an error.
