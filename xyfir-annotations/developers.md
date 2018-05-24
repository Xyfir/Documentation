Prior to reading through the developer documentation, you should first familiarize yourself with the [user help docs](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md) as well as the [affiliate docs](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/affiliates.md).

Currently only a small portion of the xyAnnotations API is publicly documented. If you need access to other parts of the API please contact us with the what you need access to and why you wish to utilize it.

# API Authentication

In order to make requests to the xyAnnotations API, you will need to authenticate with one of three different keys: `access`, `affiliate`, or `subscription`. Different keys are needed for different requests. Access keys are used for general access to the API and will be mostly ignored in this documentation for now. Affiliate keys are used for actions that only an affiliate can perform. Subscription keys are used solely for downloading annotation sets and their items.

xyAnnotations uses basic HTTP authentication, where the 'user' is the type of key, and the 'password' is the key itself. For example, a request to get your account's notifications using an access key would look like: `GET https://access:atAf5RtW...@annotations.xyfir.com/api/notifications`.

You must provide the appropriate 'user' and 'pass' with every request.

# Subscriptions

A user must have access to a Xyfir Annotations subscription in order to load annotations into your reader. A reader can purchase subscriptions directly from Xyfir Annotations or through your ebook reader system. When a user purchases a subscription they are given a subscription key that will allow them to download annotation sets.

A subscription not purchased directly through Xyfir Annotations **cannot** be extended. This means a user must wait until their subscription expires to purchase a new one (thus generating a new key) or create an entirely new subscription (thus abandoning their old key).

## Requesting a Subscription Key

To make things easier for users who have a xyAnnotations subscription key that they wish to bring to your application, you can request access to their subscription key.

Simply send the user to the following url: `https://annotations.xyfir.com/account/subscription/request`. Appended to that url should be `?redirect=REDIRECT_URL` where `REDIRECT_URL` is a url that the user will be redirected to after they approve or deny your request. The redirect url should contain the string `SUBSCRIPTION_KEY`, which will be replaced either by the user's subscription key, or by the number `0` if they decline the request.

For example if the redirect url was `https://example.com/?subscriptionKey=SUBSCRIPTION_KEY`, the user would be redirected to `https://example.com/?subscriptionKey=0` if they decline the request and `https://example.com/?subscriptionKey=THEIR_SUBSCRIPTION_KEY_HERE` if they allow it.

Remember to encode the value for the redirect variable before sending the user to the request page.

## Generating Individual Subscription Keys (ISK's)

`POST` to `https://annotations.xyfir.com/api/affiliate/subscriptions`

* `days`: _number_ - The amount of days the subscription key will last for. Currently, only the values `30` and `365` are allowed.
* `affiliate` authentication required.

### Response

```ts
{ key?: string, message?: string }
```

If the request was successful (status `200`), `key` will contain the generated subscription key. If it failed, `message` may be present to explain what went wrong.

## Using a General Subscription Key (GSK)

All affiliates receive a single GSK that can be retrieved in the affiliate panel alongside your API key. Your GSK works just the same as an ISK, except that they are not linked to individual users, and you are charged per annotation set downloaded.

While ISK's can safely be given to users, you should keep your GSK private to prevent someone from using it and leaving you with the bill.

## Deleting Subscriptions

If for whatever reason you need to delete a subscription key that you generated you can do so here. If the key has not been paid for it will be removed and you will not owe money on it.

`DELETE` to `https://annotations.xyfir.com/api/affiliate/subscriptions`

* `subscriptionKey`: _string_ - The subscription key previously generated
* `affiliate` authentication required.

### Response

```ts
{ message?: string }
```

If the request failed (non-`200` status), `message` will explain what wrong.

# Annotations

## Finding Annotation Sets

`GET` to `https://annotations.xyfir.com/api/sets`

* `search`: _string_ - A search query for finding matching annotation sets.
* `sort`: _string_ - Sort method for matched annotation sets
  * `top` - Sort by top rated sets
  * `newest` - Sort by newest created sets
  * `updated` - Sort by recently updated sets
* `direction`: _string_ - The direction to sort the sets
  * `desc` - Descending sort (greatest to least)
  * `asc` - Ascending sort (least to greatest)
* `lastId`: _number_ (optional) - Used for pagination. The last id in the previously returned list.
* `global`: _boolean_ (optional) - If true, global annotation sets will be included in results.

### Response

```js
{
  sets: [
    {
      id: Number,
      title: String,
      summary: String,
      stars: Number,
      comments: Number,
      created: Number,
      updated: Number,
      username: String,
      media: {
        books: [
          {
            id: Number,
            sets: Number,
            username: String,
            title: String,
            authors: String,
            isbn: Number,
            olCoverId: Number
          }
        ]
      }
    }
  ];
}
```

### Advanced Searches

Advanced searches allow you to provide different search queries for specific annotation set fields. All variables must be present in the query string, even if they have no value. Only sets that match **all** provided queries will be returned.

`GET` to `https://annotations.xyfir.com/api/sets`

* `sort`: _string_ - Same variable and possible values as previously explained.
* `direction`: _string_ - Same variable and possible values as previously explained.
* `lastId`: _number_ (optional) - Same variable as previously explained.
* `bookTitle`: _string_ - A search query for finding annotation sets by their books' title.
* `bookSeries`: _string_ - A search query for finding annotation sets by their books' series.
* `bookAuthors`: _string_ - A search query for finding annotation sets by their books' authors.
* `title`: _string_ - A search query for finding annotation sets by their title.
* `summary`: _string_ - A search query for finding annotation sets by their summary.
* `language`: _string_ (optional) - 3-letter language code.

## Get Annotation Set

Get the full metadata for an annotation set. Does not include its items.

`GET` to `https://annotations.xyfir.com/api/sets/:SET`

* `:SET`: _number_ - The id of the annotation set you wish to get the data for

### Response

```js
{
  id: Number,
  items: Number,
  stars: Number,
  title: String,
  global: Boolean,
  public: Boolean,
  created: Number,
  updated: Number,
  summary: String,
  changes: Number,
  starred: Boolean,
  comments: Number,
  language: String,
  comments: Number,
  description: String,
  discussion_id: Number,
  user: {
    id: Number,
    name: String,
    reputation: Number
  },
  fork: {
    id: Number,
    title: String
  },
  moderators: [{
    id: Number,
    name: String,
    reputation: Number
  }],
  media: {
    books: [{
      id: Number,
      sets: Number,
      isbn: Number,
      title: String,
      authors: String,
      username: String,
      olCoverId: Number
    }]
  }
}
```

**Note:** `description` may be in Markdown format. It is recommended to parse the Markdown to HTML or plain text, otherwise you may see formatting characters in the set's description.

## Downloading Annotation Sets

Download an annotation set's items.

`GET` to `https://annotations.xyfir.com/api/sets/:SET/download`

* `:SET`: _number_ - The id of the annotation set you wish to download
* `version`: _number_ (optional) - The version of the set that you have saved locally.
* `minify`: _boolean_ (optional) - Minify the response. Set to `1` or `true` to enable.
* `subscription` authentication required.

### Minification

You can also optionally enable minification to decrease the size of, and number of variables within the response. Enabling `minify` provides the following transformations:

* Converts boolean values (a search's `regex`) from `true` and `false` to `1` and `0`.
* Removes `regex`, `before`, and `after` values from searches if they're [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy).
* Convert search objects where `regex`, `before`, and `after` are falsy into strings containing just the `main` subsearch. For example `[{ main: 'Example', regex: false, before: '', after: '' }, ...]` becomes `['Example', ...]`.
* `title` is removed from the item object if it can be infered from the first search element's `main` subsearch. This means if `title` is missing you should retrieve it from `searches[0].main` or `searches[0]` if it's a string. The `title` prop will still be present if the item has a title different than the first search element's `main` subsearch.

Enabling `minify` can have a _huge_ impact on response size and times. They not only save bandwidth, but also decrease parse times, memory usage, and storage space in the likely event that you save these sets locally. It is _highly_ recommended that you build your application to support minified annotation set items.

### Set Versions

You can optionally provide the current version of the set that you have saved on the client. `version` is a unix timestamp (in milliseconds) of the set's last update. If the locally installed version matches the server version we won't bother returning all the info you already have. It is highly recommended you _do_ provide the version if you have downloaded and stored the set previously as our API rate limits subscription keys based on how many sets it has downloaded in a day. If a set's version has not been changed, it will not count as a download for that subscription key.

### Response

Be warned, this response can be quite large for certain annotation sets, especially without minification.

**This is the original, unminified response:**

```js
{
  set?: {
    id: Number,
    version: Number,
    items?: [{
      id: Number,
      title: String,
      searches: [{
        main: String,
        regex: Boolean,
        before: String,
        after: String
      }],
      annotations: [{
        type: Number,
        name: String,
        value: String
        // Other properties may be present based on `type`
      }]
    }]
  },
  message?: String
}
```

**This is the minified response:**

```js
{
  set?: {
    id: Number,
    version: Number,
    items?: [{
      id: Number,
      title?: String,
      searches: [
        String | {
          main: String,
          regex?: Number,
          before?: String,
          after?: String
        }
      ],
      annotations: [{
        type: Number,
        name: String,
        value: String
        // Other properties may be present based on `type`
      }]
    }]
  },
  message?: String
}
```

If the request failed (non-`200` status), the `message` property may also be present that will give you an idea of what went wrong. An error usually means that the user's subscription key is invalid, expired, or they've hit their download limit for the day.

If the annotation set that you requested has the same version as the one you already have saved locally, then the `items` property will not be present. We recommend checking the returned `version` against the locally installed version. If it's different, then you can count on `items` being there.

## Searches

Every set item contains one or more searches that are used to find content in a book to annotate. All of the set item's annotations should be applied to any content in a book that matches **any** of its searches.

### Object

**Normal:**

```js
{
  main: String,
  after: String,
  regex: Boolean,
  before: String
}
```

* `main`: _string_
  * This is the actual search query that will be searched for and highlighted within the book's content. This is either a normal string of characters or a regular expression if the search is marked as a regular expression.
* `regex`: _boolean_
  * When true, `main`, `before`, and `after` are regular expressions.
* `before`: _string_
  * This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "before" subsearch does not come _before_ the match for the "main" subsearch. This search query should only have a single match within a book's entire content.
* `after`: _string_
  * This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "after" subsearch does not come _after_ the match for the "main" subsearch. This search query should only have a single match within a book's entire content.

**Minified:**

```js
String | {
  main: String,
  after?: String,
  regex?: Number,
  before?: String
}
```

## Annotations

Every set item contains one or more annotations that will be applied anywhere one of its searches match in a book's content. A set item can have multiple annotations of the same type.

### Objects

See the various annotation types explained in the [user help docs](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md#annotations) for more information.

All annotation objects contain the following properties:

* `type`: `number` - An integer between 1 and 7.
* `name`: `string` - A short (50 characters) name/title/description that explains the annotation.

Annotation objects of different types have different properties and values:

* **(1)** Document
  * `value`: `string` - A Markdown ([CommonMark](https://spec.commonmark.org/) + [GFM](https://github.github.com/gfm/)) document string.
* **(2)** Link
  * `value`: `string` - A link that starts with `"http://"` or `"https://"`.
* **(3)** Web Search
  * `value`: `string` - A search query to be used in a search engine like Google or Bing.
  * `context`: `string` _optional_ - A value that should be appended or prepended to `value` (separated by a space) if the user chooses add context to their search. If not provided, the reader application may optionally generate one using the book's metadata. See [contextual web searches](https://github.com/Xyfir/Documentation/blob/master/xyfir-annotations/help.md#contextual-web-searches).
* **(4)** Image
  * `value`: `string|string[]` - Either a single image link or an album of images if an array. Each link will be a direct link to a JPG, JPEG, PNG, or GIF image.
* **(5)** Video
  * `value`: `string|string[]` - Either a single video ID or an array of video ID's. Each ID will point to a video on YouTube or Vimeo. You cannot mix videos from different sources.
  * `source`: `string` - The source that the video ID's belong to. Possible values: `"youtube"` or `"vimeo"`. Other sources may be added in the future.
* **(6)** Audio
  * `value`: `string|string[]` - Either a single audio track ID or an array of audio track ID's. Each ID will point to a track on SoundCloud.
  * `source`: `string` - The source that the audio ID's belong to. Possible values: `"soundcloud"`. Other sources may be added in the future.
* **(7)** Map
  * `value`: `string` - Either a link to an interactive map or a search query to be used with a real-world map like Google Maps.

## Highlighting Annotations in Content

xyAnnotations just provides the data, and from there it's up to you to determine how you want your application to highlight the annotations within a book. You have complete freedom to implement xyAnnotations into your reader however you wish, but an outline of how Xyfir Books implements the API may be helpful when developing your own implementation. The following section will give a rough outline of how xyBooks uses the annotations returned from the api. xyBooks' implementation is not the perfect or recommended way, especially since it only works with book formats that render to HTML. There are many flaws and limitations with its configuration, but for now it can serve as a pointer in the right direction.

### Resources

Take a look at Xyfir's [annotate](https://github.com/Xyfir/annotate) packages for libraries and examples for inserting and viewing annotations from xyAnnotations within content of various formats. What is described below is xyBooks' own implementation of the [@xyfir/annotate-epubjs](https://github.com/Xyfir/annotate/tree/master/epubjs) and [@xyfir/annotate-react](https://github.com/Xyfir/annotate/tree/master/react) packages that also explains some of the lower-level techniques used by the `@xyfir/annotate-*` packages themselves.

Should you wish to save yourself some work, you can also utilize the built in annotation viewer within xyAnnotations by opening the following link whenever a user clicks a highlight within a book in your reader: `https://annotations.xyfir.com/sets/:set/items/:item?view=true`. You can send the user to this link within their preferred browser, display it within an in-app browser, or even show it in an iframe. `:set` is the id of the annotation set, and `:item` is the id of the item to view.

### Step 1: Downloading Annotations

The first step is of course downloading the annotations themselves. Annotations are downloaded at two different times: when a book is opened and when a user clicks 'Save' on an annotation set in the 'Manage Annotations' section. In the first situation xyBooks checks for annotations stored in local storage for the book that is being opened. If annotations are stored for the book, then xyBooks uses the id and version for the stored annotation sets and calls xyAnnotations to get an updated version of those annotation sets.

### Step 2: Building 'Markers' for Specific Searches

Since xyAnnotations is built to support all types of ebook formats and multiple versions of any single ebook, there are no specified locations for where a set item's search can be found within an ebook's content. This means that we must search the book's content in its entirety to highlight everywhere a set item's search matches. This is straightforward for global searches that can match anywhere in a book, but dealing with non-global (specific) searches is a little more difficult.

xyBooks handles specific searches by looping through every single search within every single set item and filtering out all global searches. From here, xyBooks begins to build a `markers` object that contains keys whose names follow a format of: `${itemId}-${searchIndex}-${type}`. `itemId` is the unique id of the set item; `searchIndex` is the index of the specific search within the item's searches array; and `type` can be `1` for a 'before' marker and `2` for an 'after' marker. These keys point to an object that contains two properties: `chapter` and `index` where `chapter` is the index of the chapter the marker is present in and `index` is an index within the chapter's HTML string where the marker is present.

For example, if we have an object at `markers['5-0-1']` that contains `{ chapter: 4, index: 1992 }`:

This means that item `5`'s search at array index `0` has a `before` that is present within the book at the chapter with an index of `4` and within the chapter's HTML string at index `1992`. This data can now be used later when we need to determine whether a search's match should be highlighted.

### Step 3: Sorting Searches By Length

xyBooks only supports EPUB format ebooks, which are rendered to HTML. xyBooks then manipulates this HTML to insert highlights by wrapping matching text with a `span` element. Due to how xyBooks handles highlighting, there is a potential problem where a search that matches a small amount of text will prevent a longer section of text from being matched because of the addition of span elements into the HTML.

To partially solve this issue, xyBooks sorts searches based on length, so that the longest searches are ran first. This is done by looping through all of a set's items and then through all of an item's searches, comparing the length of the search object's `main` property, and building an array of the following object: `{ item, search }`. `item` is the id of the item and `search` is the index of the search within the item's searches array.

xyBooks also ensures that specific (has `before` and/or `after`) searches are sorted before global searches. So the order is longest specific searches -> shortest specific searches -> longest global searches -> shortest global searches.

Now, xyBooks can loop through this new array and load the appropriate item and search as needed.

### Step 4: Running Searches, Wrapping Matches

At this point we have two important variables that will now be used: `markers` and `searches`.

xyBooks starts by looping through the `searches` array and then loading the appropriate search from the specified item and search index. xyBooks first runs a search using Regular Expressions on the current chapter's HTML string using `search.main`. Remember to [escape regex characters](https://github.com/sindresorhus/escape-string-regexp) in `search.main` if `search.regex` is `false`/`undefined`!

If there were any matches, xyBooks then checks for both `search.before` and `search.after`. If either of those variables are truthy, it loads the corresponding objects from the `matches` object.

Now it loops through the matches returned from the regex search and gets their start and end indexes within the current chapter's HTML string. It throws out any matches that come _before_ `search.before` or _after_ `search.after` if the search is not global.

The matches that are left are now wrapped in a `span` element with a class name of `annotation` and an `onclick` attribute that calls a function that is passed the annotation set item's id.

xyBooks now updates the chapter's HTML and moves on to the next loop of the `searches` array.

### Step 5: Handling Highlight Clicks

A function is setup that accepts an `item` argument which is the id of an item within the active annotation set. When this function is called, it pulls the `annotations` object array for the item from the annotation set and opens a fullscreen overlay that allows the user to view the item's annotations.

xyBooks must also account for an issue that is in a ways the opposite of the issue described in Step 3. Since the longest searches are matched first, it is possible that a highlight might be _within_ another highlight. xyBooks does not differentiate in the styling of highlights that exist within other highlights, so the user does not know that they might be clicking on a highlight different than what they believe they're clicking on. To solve this issue, xyBooks currently only shows the deepest clicked highlight, however in the future will offer a popup menu that allows the user to select the item they want to view when they click/tap on a nested highlight. That popup should only be present if a nested highlight is being selected.

### Step 6: Removing Highlights

Should the user want to disable annotation highlights or switch to another annotation set, the current highlights must first be removed. There are many ways to do this but xyBooks does it by simply replacing the chapter's HTML with the unmodified HTML that was saved _before_ the annotations were inserted.
