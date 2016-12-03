# Integrating Libyq Annotations

## Subscriptions

A user must have access to a Libyq Annotations subscription in order to load annotations into your reader. A reader can purchase subscriptions directly from Libyq Annotations or through your ebook reader system. When a user purchases a subscription they are given a subscription key that will allow them to download annotation sets.

A subscription not purchased directly through Libyq Annotations **cannot** be extended. This means a user must wait until their subscription expires to purchase a new one (thus generating a new key) or create an entirely new subscription (thus abandoning their old key).

Your reader application must allow a user to:
- Purchase a subscription key through your service (you can charge any price you wish)
- Enter in a subscription key that was purchased directly through Libyq Annotations or another affiliate

### Subscription Pricing
As an added incentive for our affiliates we offer the ability for our affiliates to generate subscriptions at a discount. The discount you receive is based on how many subscriptions you have previously generated (and then paid for). Your subscription discount is 1% for every 100 generated subscriptions paid for with a minimum of 10% and a maximum of 25%.

This discount allows you to either:
- Mark up prices and make a profit
- Sell subscriptions at a discount and break even
- Sell subscriptions at a slight discount and still make a small profit

#### Base Prices
Your discount will be applied to these base prices.
- **One Month** (30 Days) - $1.50
- **One Year** (365 Days) - $12.00


### Generating Subscriptions
`POST https://annotations.libyq.com/api/affiliate/<AFFILIATE_ID>/<AFFILIATE_KEY>?subscription=<SUBSCRIPTION>`
- `AFFILIATE_ID`: *number* - Your affiliate ID found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `AFFILIATE_KEY`: *string* - Your service's api key, generated and found in your [affiliate panel](https://annotations.libyq.com/app/#/affiliate)
- `SUBSCRIPTION`: *number* - A numerical identifier for the subscription length. **1** = 30 days, **2** = 365 days

#### Response
```js
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
```js
{ error: boolean }
```

## Annotations
Our annotation system is broken down into repositories called *annotation sets*. An *annotation set* is a set of items that contain annotations for a specific book. A *set item* is further broken down into *searches* and *annotations*. A *search* is a search query that points to a set of text within a book's content. An *annotation* is the specific annotation that will be applied to the book's content matched from the *search*.

### Finding Annotation Sets
`GET https://annotations.libyq.com/api/sets?search=<SEARCH>&sort=<SORT>`
- `SEARCH`: *string* - A search query for finding matching annotation sets. This can be a set title, a book title, or book authors.
- `SORT`: *string* - Sort method for matched annotation sets
    - `top` - Sort by top rated sets
    - `newest` - Sort by newest created sets
    - `updated` - Sort by recently updated sets

#### Response
```js
{ sets: [{
    id: number, book_title: string, book_authors: string, set_title: string,
    set_description: string, votes: number
}] }
```

**Note:** `set_description` may be in Markdown format. It is recommended to parse the Markdown to HTML or plain text. Otherwise you may see formatting characters in the set's description.

#### Advanced Searches
Advanced searches allow you to provide different search queries for specific annotation set fields. All variables must be present in the query string, even if they have no value. Only sets that match **all** provided queries will be returned.

`GET https://annotations.libyq.com/api/sets?bookTitle=<BOOK_TITLE>&setTitle=<SET_TITLE>&bookAuthors=<BOOK_AUTHORS>sort=<SORT>`
- `SORT`: *string* - Same variable and possible values as previously explained.
- `BOOK_TITLE`: *string* - A search query for finding annotation sets by their book's title.
- `BOOK_AUTHORS`: *string* - A search query for finding annotation sets by their book's authors.
- `SET_TITLE`: *string* - A search query for finding annotation sets by their title.

### Downloading Annotation Sets
`GET https://annotations.libyq.com/api/annotations?key=<KEY>&sets=<SETS>`
- `KEY`: *string* - The user's subscription key
- `SETS`: *string* - The annotation sets of which you wish to download the set items from. Format: `set_id|set_version,set_id,...`. You may request up to **3** annotation sets per request.

#### Set Versions
In `SETS` you can optionally provide the current version of the set that you have saved on the client. If the locally installed version matches the server version we won't bother returning all the info you already have. It is highly recommended you *do* provide the version if you have downloaded and stored the set previously as our API rate limits subscription keys based on how many sets it has downloaded in a day. If a set's version has not been changed, it will not count as a download for that subscription key.

#### Response
```js
{ error: boolean, message?: string, "id"?: {
    version: date-string, items?: [{
        id: number, title: string,
        searches: [{
            text: string, regex: boolean, range: {
                global: boolean, before: string, after: string
            }
        }], annotations: [{
            type: number, name: string, value: string
        }]
    }]
}, ... }
```

If `error` is true, a `message` property may also be present that will give you an idea of what went wrong. An error usually means that the user's subscription key is invalid, expired, or they've hit their download limit for the day.

`"id"` is a placeholder for the ids of the requested annotation sets. This means if you requested the annotations for sets with the ids of `5,10,15` then the response would be `{error: false, "5": { ... }, "10": { ... }, "15": { ... }}`.

If an annotation set you requested has the same version as you have downloaded locally then the `items` property will not be present. We recommend checking the returned `version` against the locally installed version. If it's different then you can count on `items` being there.

### Searches
Every set item contains one or more searches that are used to find content in a book to annotate. All of the set item's annotations should be applied to any content in a book that matches **any** of its searches.

#### Object
```js
text: string, regex: boolean, range: {
    global: boolean, before: string, after: string
}
```

- `text`: *string* - This is value to search for in a book's content.
- `regex`: *boolean* - When true, `text` is a regular expression. Does not apply to `range` properties.
- `range`: *object* - This object tells you the scope of the search.
- `range.global`: *boolean* - When true, `text` should be searched for anywhere in the book's content.
- `range.before`: *string* - If this property is not an empty string, it means that any matches for this search are only valid if they come *before* where this text is present in the book's content.
- `range.after`: *string* - If this property is not an empty string, it means that any matches for this search are only valid if they come *after* where this text is present in the book's content.

### Annotations
Every set item contains one or more annotations that will be applied anywhere one of its searches matches in a book's content. A set item can have multiple annotations of the same type.

#### Types
1. Document
    - Documents are Markdown format text.
2. Link
    - An `HTTP` or `HTTPS` link.
3. Search
    - A search query to be used with a search engine (Google, Bing, etc).
4. Image
    - A link to an image of no specific format.
5. Video
    - A link to a video. Might be a YouTube, Vimeo, etc link or a direct link to a video file.
6. Audio
    - A link to audio content.
7. Map
    - Can be a direct link to a map of any format (image, interactive, etc).
    - Can be a search query for use in a real-world map (Google Maps, Bing Maps, etc).

#### OBJECT
```js
// -- DOCUMENT --
{ type: 1, name: "Document", value: "A **document** annotation that *supports* [Markdown](https://en.wikipedia.org/wiki/Markdown)." }
// -- LINK --
{ type: 2, name: "Link", value: "https://en.wikipedia.org/wiki/SomeWikipediaLink" }
// -- SEARCH --
{ type: 3, name: "Search", value: "some search value" }
// -- IMAGE --
{ type: 4, name: "Image", value: "https://xyfir.com/images/SomeImage.jpg" }
// -- VIDEO --
{ type: 5, name: "Video", value: "https://www.youtube.com/embed/SomeYouTubeVideo" }
// -- AUDIO --
{ type: 6, name: "Audio", value: "https://xyfir.com/audio/SomeFile.mp3" }
// -- MAP --
{ type: 7, name: "Map", value: "San Diego, CA" }
// -- MAP --
{ type: 7, name: "Special Map", value: "https://xyfir.com/map/SomeMap" }
```
