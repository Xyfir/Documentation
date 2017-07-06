Prior to reading through the integration documentation, you should first familiarize yourself with Xyfir Annotations' [user help docs](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md).

# Subscriptions

A user must have access to a Xyfir Annotations subscription in order to load annotations into your reader. A reader can purchase subscriptions directly from Xyfir Annotations or through your ebook reader system. When a user purchases a subscription they are given a subscription key that will allow them to download annotation sets.

A subscription not purchased directly through Xyfir Annotations **cannot** be extended. This means a user must wait until their subscription expires to purchase a new one (thus generating a new key) or create an entirely new subscription (thus abandoning their old key).

Your reader application must allow a user to:
- Purchase a subscription key through your service (you can charge any price you wish)
- Enter in a subscription key that was purchased directly through Xyfir Annotations or another affiliate

## Subscription Pricing

As an added incentive for our affiliates we offer the ability for our affiliates to generate subscriptions at a discount. The discount you receive is based on how many subscriptions you have previously generated (and then paid for). Your subscription discount is 1% for every 100 generated subscriptions paid for with a minimum of 10% and a maximum of 25%.

This discount allows you to either:
- Mark up prices and make a profit
- Sell subscriptions at a discount and break even
- Sell subscriptions at a slight discount and still make a small profit

### Base Prices

Your discount will be applied to these base prices.
- **One Month** (30 Days) - $1.50
- **One Year** (365 Days) - $12.00


## Generating Subscriptions

`POST https://annotations.xyfir.com/api/affiliate/subscriptions`
- `affiliateId`: *number* - Your affiliate ID found in your [affiliate panel](https://annotations.xyfir.com/app/#/affiliate)
- `affiliateKey`: *string* - Your service's api key, generated and found in your [affiliate panel](https://annotations.xyfir.com/app/#/affiliate)
- `subscription`: *number* - A numerical identifier for the subscription length. **1** = 30 days, **2** = 365 days

### Response

```ts
{ error: boolean, key?: string }
```
If the request was successful and `error` is false, `key` will contain the generated subscription key.

## Paying for Subscriptions

You can generate subscriptions anytime you want for free. However, if you want those subscriptions to last, they must be paid for. You can pay off generated subscriptions at any time in your [affiliate panel](https://annotations.xyfir.com/app/#/affiliate). Subscriptions that you generate will be deleted after 8 days if they have not been paid for. It is recommended that you pay for your generated subscriptions within 7 days to prevent a user's subscription key from being deleted.

Affiliates found exploiting our subscription system will be banned.

## Deleting Subscriptions

If for whatever reason you need to delete a subscription key that you generated you can do so here. If the key has not been paid for it will be removed and you will not owe money on it.

`DELETE https://annotations.xyfir.com/api/affiliate/subscriptions`
- `affiliateId`: *number* - Your affiliate ID found in your [affiliate panel](https://annotations.xyfir.com/app/#/affiliate)
- `affiliateKey`: *string* - Your service's api key, generated and found in your [affiliate panel](https://annotations.xyfir.com/app/#/affiliate)
- `subscriptionKey`: *string* - The subscription key previously generated

### Response

```ts
{ error: boolean }
```

# Annotations

## Finding Annotation Sets

`GET https://annotations.xyfir.com/api/sets?search=<SEARCH>&sort=<SORT>&direction=<DIRECTION>&lastId=<LAST_ID>`
- `SEARCH`: *string* - A search query for finding matching annotation sets. This can be a set title, a book title, or book authors.
- `SORT`: *string* - Sort method for matched annotation sets
  - `top` - Sort by top rated sets
  - `newest` - Sort by newest created sets
  - `updated` - Sort by recently updated sets
- `DIRECTION`: *string* - The direction to sort the sets
  - `desc` - Descending sort (greatest to least)
  - `asc` - Ascending sort (least to greatest)
- `LAST_ID`: *number* (optional) - Used for pagination. The last id in the previously returned list.


### Response

```ts
{
  sets: [{
    id: number, book_title: string, book_authors: string, set_title: string,
    set_description: string, votes: number
  }]
}
```

**Note:** `set_description` may be in Markdown format. It is recommended to parse the Markdown to HTML or plain text. Otherwise you may see formatting characters in the set's description.

### Advanced Searches

Advanced searches allow you to provide different search queries for specific annotation set fields. All variables must be present in the query string, even if they have no value. Only sets that match **all** provided queries will be returned.

`GET https://annotations.xyfir.com/api/sets?bookTitle=<BOOK_TITLE>&setTitle=<SET_TITLE>&bookAuthors=<BOOK_AUTHORS>&sort=<SORT>&direction=<DIRECTION>&lastId=<LAST_ID>`
- `SORT`: *string* - Same variable and possible values as previously explained.
- `DIRECTION`: *string* - Same variable and possible values as previously explained.
- `LAST_ID`: *number* (optional) - Same variable as previously explained.
- `BOOK_TITLE`: *string* - A search query for finding annotation sets by their book's title.
- `BOOK_AUTHORS`: *string* - A search query for finding annotation sets by their book's authors.
- `SET_TITLE`: *string* - A search query for finding annotation sets by their title.

## Downloading Annotation Sets

`GET https://annotations.xyfir.com/api/annotations?subscriptionKey=<KEY>&sets=<SETS>`
- `KEY`: *string* - The user's subscription key
- `SETS`: *json-string* - The annotation sets you wish to download set items from. The value should be a JSON string of an object array: `[{ id: number, version?: date-string }]`. You may request up to **3** annotation sets per request.

### Set Versions

In `SETS` you can optionally provide the current version of the set that you have saved on the client. If the locally installed version matches the server version we won't bother returning all the info you already have. It is highly recommended you *do* provide the version if you have downloaded and stored the set previously as our API rate limits subscription keys based on how many sets it has downloaded in a day. If a set's version has not been changed, it will not count as a download for that subscription key.

### Response

```ts
{
  error: boolean, message?: string,
  [setId]?: {
    version: date-string,
    items?: [{
      id: number, title: string,
      searches: [{
        text: string, regex: boolean,
        range: {
          global: boolean, before?: string, after?: string
        }
      }],
      annotations: [{
        type: number, name: string, value: string
      }]
    }]
  },
  [setId]?: { },
  [setId]?: { }
}
```

If `error` is true, a `message` property may also be present that will give you an idea of what went wrong. An error usually means that the user's subscription key is invalid, expired, or they've hit their download limit for the day.

`[setId]` is a placeholder for the ids of the requested annotation sets. This means if you requested the annotations for sets with the ids of `5,10,15` then the response would be `{error: false, "5": { ... }, "10": { ... }, "15": { ... }}`.

If an annotation set you requested has the same version as you have downloaded locally then the `items` property will not be present. We recommend checking the returned `version` against the locally installed version. If it's different, then you can count on `items` being there.

## Searches

Every set item contains one or more searches that are used to find content in a book to annotate. All of the set item's annotations should be applied to any content in a book that matches **any** of its searches.

### Object

```ts
text: string, regex: boolean, range: {
  global: boolean, before?: string, after?: string
}
```

- `text`: *string* - This is value to search for in a book's content.
- `regex`: *boolean* - When true, `text`, `range.before`, and `range.after` are regular expressions.
- `range`: *object* - This object tells you the scope of the search.
- `range.global`: *boolean* - When true, `text` should be searched for anywhere in the book's content. If false, `range.before` and `range.after` should be checked.
- `range.before`: *string* (optional) - If this property is truthy, it means that any matches for this search are only valid if they come *before* where this text is present in the book's content.
- `range.after`: *string* (optional) - If this property is truthy, it means that any matches for this search are only valid if they come *after* where this text is present in the book's content.

## Annotations

Every set item contains one or more annotations that will be applied anywhere one of its searches match in a book's content. A set item can have multiple annotations of the same type.

### Objects

All annotation objects contain the following properties:

- **type**: *number* - A number between 1 and 7. See [user help docs](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md#annotations).
- **name**: *string* - A short name/title/description that explains the annotation.

Annotation objects of different types have different properties and values:

- (1) Document
  - **value**: *string* - A [Markdown](https://en.wikipedia.org/wiki/Markdown) document string.
- (2) Link
  - **value**: *string* - A link that starts with 'http' or 'https'.
- (3) Web Search
  - **value**: *string* - A search query to be used in a search engine like Google or Bing.
  - **context**: *string* (optional) - A value that should be appended or prepended to `value` (separated by a space) if the user chooses add context to their search. See [contextual web searches](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md#contextual-web-searches).
- (4) Image
  - **value**: *string* OR *string[]* - Either a single image link or an album of images if an array.
- (5) Video
  - **value**: *string* OR *string[]* - Either a single video link or a playlist of videos if an array. Each link *should* be either a direct video link or an embed link for YouTube, Vimeo, etc.
- (6) Audio
  - **value**: *string* OR *string[]* - Either a single audio file link or a playlist of audio files if an array. Each link *should* be either a direct link to an audio file.
- (7) Map
  - **value**: *string* - Either a link to a map of any format (image, interactive, etc) or a search query to be used with a real-world map like Google Maps.

## Highlighting Annotations

xyAnnotations just provides the data, and from there it's up to you to determine how you want your application to highlight the annotations within a book. You have complete freedom to implement xyAnnotations into your reader however you wish, but an outline of how Xyfir Books implements the api may be helpful when developing your own implementation. The following section will give a rough outline of how xyBooks uses the annotations returned from the api. xyBooks' implementation is not the perfect or recommended way, especially since it only works with book formats that render to HTML. There are many flaws and limitations with its configuration, but for now it can serve as a pointer in the right direction.

### Step 1: Downloading Annotations

The first step is of course downloading the annotations themselves. Annotations are downloaded at two different times: when a book is opened and when a user clicks 'Save' on an annotation set in the 'Manage Annotations' section. In the first situation xyBooks checks for annotations stored in local storage for the book that is being opened. If annotations are stored for the book, then xyBooks uses the id and version for the stored annotation sets and calls xyAnnotations to get an updated version of those annotation sets.

### Step 2: Building 'Markers' for Non-Global Searches

Since xyAnnotations is built to support all types of ebook formats and multiple versions of any single ebook, there are no specified locations for where a set item's search can be found within an ebook's content. This means that we must search the book's content in its entirety to highlight everywhere a set item's search matches. This is straightforward for global searches that can match anywhere in a book, but dealing with non-global searches is a little more difficult.

xyBooks handles non-global searches by looping through every single search within every single set item and filtering out all global searches. From here, xyBooks begins to build a `markers` object that contains keys whose names follow a format of: `${itemId}-${searchIndex}-${type}`. `itemId` is the unique id of the set item; `searchIndex` is the index of the specific search within the item's searches array; and `type` can be `1` for a 'before' marker and `2` for an 'after' marker. These keys point to an object that contains two properties: `chapter` and `index` where `chapter` is the index of the chapter the marker is present in and `index` is an index within the chapter's HTML string where the marker is present.

For example, if we have an object at `markers['5-0-1']` that contains `{ chapter: 4, index: 1992 }`:

This means that item `5`'s search at array index `0` has a `range.before` that is present within the book at the chapter with an index of `4` and within the chapter's HTML string at index `1992`. This data can now be used later when we need to determine whether a search's match should be highlighted.

### Step 3: Sorting Searches By Length

xyBooks only supports EPUB format ebooks, which are rendered to HTML. xyBooks then manipulates this HTML to insert highlights by wrapping matching text with a `span` element. Due to how xyBooks handles highlighting, there is a potential problem where a search that matches a small amount of text will prevent a longer section of text from being matched because of the addition of span elements into the HTML.

To handle this issue, xyBooks sorts searches based on length, so that the longest searches are ran first. This is done by looping through all of a set's items and then through all of an item's searches, comparing the length of the search object's `text` property, and building an array of the following object: `{ item, search }`. `item` is the id of the item and `search` is the index of the search within the item's searches array.

Now, xyBooks can loop through this new array and load the appropriate item and search as needed.

### Step 4: Running Searches, Wrapping Matches

At this point we have two important variables that will now be used: `markers` and `searches`.

xyBooks starts by looping through the `searches` array and then loading the appropriate search from the specified item and search index. xyBooks first runs a search using Regular Expressions on the current chapter's HTML string using `search.text`. Remember to escape regex characters in `search.text` if `search.regex` is false!

If there were any matches, xyBooks then checks if `search.range.global` is false and if so checks for both `search.range.before` and `search.range.after`. If either of those variables are truthy, it loads the corresponding objects from the `matches` object.

Now it loops through the matches returned from the regex search and gets their start and end indexes within the current chapter's HTML string. It throws out any matches that come *after* `search.range.before` or *before* `search.range.after` if `search.range.global` is true.

The matches that are left are now wrapped in a `span` element with a class name of `annotation` and an `onclick` attribute that calls a function that is passed the annotation set item's id.

xyBooks now updates the chapter's HTML and moves on to the next loop of the `searches` array.

### Step 5: Handling Highlight Clicks

A function is setup that accepts an `item` argument which is the id of an item within the active annotation set. When this function is called, it pulls the `annotations` object array for the item from the annotation set and opens a fullscreen overlay that allows the user to view the item's annotations.

xyBooks must also account for an issue that is in a ways the opposite of the issue described in Step 3. Since the longest searches are matched first, it is possible that a highlight might be *within* another highlight. xyBooks does not differentiate in the styling of highlights that exist within other highlights, so the user does not know that they might be clicking on a highlight different than what they believe they're clicking on. To solve this issue, xyBooks passes the click *up* to any parent element that is a `span` element with the class `annotation`.

### Step 6: Unwrapping Highlights

Should the user want to disable annotation highlights or switch to another annotation set, the current highlights must be removed. There are many ways to do this but xyBooks does it by 'unwrapping' all `span` elements within the chapter's HTML that has a class of `annotation`. The content within a `span.annotation` element is pulled out and put into its parent element. Afterward the `span.annotation` element is removed from the DOM.