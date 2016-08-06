# Integrating Libyq Annotations

## Subscriptions

A user must have access to a Libyq Annotations subscription in order to load annotations into your reader. A reader can purchase subscriptions directly from Libyq Annotations or through your ebook reader system. When a user purchases a subscription they are given a subscription key that will allow them to download annotation sets.

A subscription not purchased directly through Libyq Annotations **cannot** be extended. This means a user must wait until their subscription expires to purchase a new one (thus generating a new key) or create an entirely new subscription (thus abandoning their old key).

Your reader application must allow a user to:
- Purchase a subscription key through your service (you can charge any price you wish)
- View their purchased subscription key
- Enter in a subscription key that was purchased directly through Libyq Annotations (or another affiliate)

### Subscription Pricing
As an added incentive for our affiliates we offer the ability for our affiliates to generate subscriptions at a discount. The discount you receive is based on how many subscriptions you have previously generated (and then paid for). Your subscription discount is 1% for every 100 generated subscriptions paid for with a minimum of 10% and a maximum of 25%.

This discount allows you to either:
- Mark up prices and make a profit
- Sell subscriptions at a discount and break even
- Sell subscriptions at a slight discount and still make a small profit

#### Base Prices
Your discount will be applied to these base prices.
- **One Month** (30 Days) - $1
- **One Year** (365 Days) - $10


### Generating Subscriptions
`POST https://annotations.libyq.com/api/affiliate/<AFFILIATE_ID>/<AFFILIATE_KEY>?subscription=<SUBSCRIPTION>`
- `AFFILIATE_ID`: *number* - Your affiliate ID found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `AFFILIATE_KEY`: *string* - Your service's api key, generated and found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `SUBSCRIPTION`: *number* - A numerical identifier for the subscription length. **1** = 30 days, **2** = 365 days

#### Response
```
{ error: boolean, key?: string }
```
If the request was successful and `error` is false, `key` will contain the generated subscription key.

### Paying for Subscriptions
You can generate subscriptions anytime you want for free. However, if you want those subscriptions to last, they must be paid for. You can pay off generated subscriptions at any time in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate). Subscriptions that you generate will be deleted after 8 days if they have not been paid for. It is recommended that you pay for your generated subscriptions within 7 days to prevent a user's subscription key from being deleted.

Affiliates found exploiting our subscription system will be banned.

### Deleting Subscriptions
If for whatever reason you need to delete a subscription key that you generated you can do so here. If the key has not been paid for it will be removed and you will not owe money on it.

`DELETE https://annotations.libyq.com/api/affiliate/<AFFILIATE_ID>/<AFFILIATE_KEY>/<SUBSCRIPTION_KEY>`
- `AFFILIATE_ID`: *number* - Your affiliate ID found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `AFFILIATE_KEY`: *string* - Your service's api key, generated and found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `SUBSCRIPTION_KEY`: *string* - The subscription key previously generated

#### Response
```
{ error: boolean }
```

## Annotations
Our annotation system is broken up into *annotation sets*. An annotation set is a set of annotations for a specific book. The annotations within a set we call a *set item*.

### Finding Annotation Sets
`GET https://annotations.libyq.com/api/sets?search=<SEARCH>&sort=<SORT>`
- `SEARCH`: *string* - A search query for finding matching annotation sets. Usually this is the set name, a book title, or book authors.
- `SORT`: *string* - Sort method for matched annotation sets
    - `newest` - Sort by newest created sets
    - `top` - Sort by top rated sets
    - `updated` - Sort by recently updated sets

#### Response
```
{
    sets: [{
        id: number, book_title: string, book_authors: string, set_title: string,
        set_description: string, votes: number
    }]
}
```

**Note:** `set_description` may be in Markdown format. It is recommended to parse the Markdown to HTML or plain text. Otherwise you may see formatting characters in the set's description.

### Downloading Annotation Sets
`GET https://annotations.libyq.com/api/annotations?key=<KEY>&sets=<SETS>`
`KEY`: *string* - The user's subscription key
`SETS`: *string* - The annotation sets of which you wish to download the set items from. Format: `set_id|set_version,set_id,...`

#### Set Versions
In `SETS` you can optionally provide the current version of the set that you have saved on the client. If the locally installed version matches the server version we won't bother returning all the info you already have. It is highly recommended you *do* provide the version if you have downloaded and stored the set previously as our API rate limits subscription keys based on how many sets it has downloaded in a day. If a set's version has not been changed, it will not count as a download for that subscription key.

#### Response
```
{ error: boolean, "id"?: {
    version: date-string, items?: [{
        id: number, title: string, object: json-string
    }]
}, ... }
```

If `error` is true this usually means that the user's subscription key is invalid, expired, or they've hit their request limit for the day. `"id"` is a placeholder for the ids of the requested annotation sets. This means if you requested the annotations for sets with the ids of `5,10,15` then the response would be `{error: false, "5": { ... }, "10": { ... }, "15": { ... }}`.

If an annotation set you requested has the same version as you have downloaded locally then the `items` property will not be present. We recommend checking the returned `version` against the locally installed version. If it's different you can count on `items` being there.