# Terminology

Xyfir Annotations' annotation system is broken down into repositories called `annotation sets`. An `annotation set` is a set of `items` that contain annotations for a specific book. A `set item` is further broken down into `searches` and `annotations`. A `search` is a search query that points to text within a book's content. An `annotation` is the specific annotation that will be applied to the book's content matched from the `search`. A `set item` can have multiple `searches` and multiple `annotations`.

In certain contexts `annotations` may be used to refer to all of the items within an `annotation set`.

Continue reading for more in-depth descriptions.

# Annotation Sets

An annotation set is a central container for set items, change proposals, and discussions for a single book. An annotation set typically has a 'goal' specified in its description that is set by its creator and moderators. This goal should dictate what type of annotations the set contains and should drive the annotation set's evolution in the future into a specific direction. Because a set can serve so many different purposes, it is important that a goal is set early and is explained clearly in the set's description so that potential contributors can know how to help. Ideas for annotation set goals are: explanations of things referenced in the book (people, places, etc); criticism; translations from one language to another; background information and insights; etc.

## Moderators

You can make other users moderators to your annotation set to help with the management of your set. Moderators have all of the special powers that you as a creator have but they can lose their powers if removed from the moderator list while you can never be removed as a creator.

### Global Moderator

A 'Global Moderator' is a user that has moderator powers across the entire site and has full power over all annotation sets regardless of whether they were made a moderator or not. A global mod can also delete annotation sets, something that normal moderators and even a set creator cannot do.

## Items

Annotation set items typically encompass a single part of an annotation set. An item should contain searches that are all relevant to each other while the annotations should all be relevant to the item's searches and should accomplish the goal of the annotation set as a whole.

### Searches

A set item's searches point to sections of a book where the item's annotations should be applied. A search can be any portion of text within a book's content.

Searches contain the following values:

- **Search Query**: This is the actual search query that will be looked for to be highlighted within the book's content. This is either a normal string of characters or a regular expression if the search is marked as a regular expression.
- **Regular Expression**: This is a true/false value that tells whether the search query and range (before/after) queries are just normal strings or regular expressions.
- **Only Match If Before**: This is another search query (string or regular expression) that prevents a potential match for the main search query from matching if it's position within the book does not come before the instance of where the match for this range search query is. This search query should only have a single match within a book's content.
- **Only Match If After**: This is another search query (string or regular expression) that prevents a potential match for the main search query from matching if it's position within the book does not come after the instance of where the match for this range search query is. This search query should only have a single match within a book's content.
- **Global**: A search is considered 'global' if there are no 'only match if before|after' search queries. A global search can match anywhere within a book.

### Annotations

A set item can contain multiple annotations of the same or different types. Annotations should explain the content that the item's searches match in a way that is relevant to the goal of the annotation set.

There are multiple annotation types:

- (1) Document
  - Documents are Markdown format text.
- (2) Link
  - An `HTTP` or `HTTPS` link.
- (3) Web Search
  - A web search query to be used with a search engine (Google, Bing, etc).
  - Not to be confused with an annotation set item's *searches*.
  - Supports optional [contextual web searches](#contextual-web-searches).
- (4) Image
  - A link to an image of no specific format.
  - If multiple links are provided it becomes an album.
- (5) Video
  - A link to a video of no specific format.
  - Might be a YouTube, Vimeo, etc embed link or a direct link to a video file.
  - If multiple links are provided it becomes a playlist.
- (6) Audio
  - A direct link to an audio file.
  - If multiple links are provided it becomes a playlist.
- (7) Map
  - Can be a direct link to a map of any format (image, interactive, etc).
  - Can be a search query for use in a real-world map (Google Maps, Bing Maps, etc).

#### Contextual Web Searches

This feature allows you to add context to a 'Web Search' annotation. When a reader views an annotation that has a context value, they can optionally enable the context which will add the 'Context Search' value to the normal web search query. Typically, this is the book title, series, or some other text that can be used to narrow down the scope of possible results to increase relevancy. Imagine you're creating annotations for a historical fiction book and you're creating an annotation for the text 'George Washington'. You might want the search query to default to just his name while allowing an optional search that adds the name of the book title.

### Generating Annotations

xyAnnotations has a built-in feature for easily generating simple searches and annotations by using a book's text content. The generator is built to parse the book's content and pull out proper nouns (a specific person, place, or thing). This works for the most part because in English (and some other languages) proper nouns are capitalized in the middle of sentences while nothing else is. By utilizing this rule, xyAnnotations can usually get most of the things that might be interesting and worthy of creating basic 'Web Search' annotations for. This system is definitely not perfect, but if you're looking to quickly pull proper nouns out of a book and create basic set items for them then this feature should be satisfactory.

If you populate an annotation set strictly with items from this feature and you make little to no changes to the generated items it is recommended to title your set 'Generated Annotations' or at least note in the description that all the items were automatically generated so that other users know what to expect.

A fully automated solution is available if you wish to automatically generate annotations for an entire library of ebooks. See [auto-annotator](https://github.com/Xyfir/auto-annotator).

#### 'Found Items' Section

This section contains any items that our system thinks is a proper noun.

By clicking **Add**, that item will be removed from the *Found Items* section and added to the *Create Item* section.

By clicking **Remove**, that item will be removed from the *Found Items* section and the only way to get it back is to start the generation process over again.

Add items you wish to use as searches for a set item and remove items that already have a set item, aren't relevant, or you simply wish to ignore.

You should add multiple items to the *Create Item* section that are simply different versions of the same proper noun.

#### 'Create Item' Section

This section contains any items that you clicked **Add** on in the *Found Items* section. This section allows you to use those items as searches for automatically generated annotations. Together, the searches and their generated annotations will create a single set item.

Removing an added item will only remove it from the *Create Item* section and will return it to the *Found Items* section.

**Selecting an item for use in generating annotations:** While you can add multiple items as searches, only one item in that list can be selected to be used in the annotations themselves. To select an item, simply click its text in the *Create Item* section. If an item's text is bold, it will be used in creating the annotations you choose and it will also be used as the title for the set item.

**Generating a Web Search annotation:** By checking the box for *Web Search Annotation* our system will automatically create a search annotation using the item's text and optionally the book's title as a search query (for Google, Bing, etc).

**Generating a map annotation:** By checking the box for *Map Annotation* our system will automatically create a map annotation using the item's text as a search query (for Google Maps, etc). Enable map annotation generation if your selected item's text is a place that can be found on an actual map (a place, city, zip code, etc).

**What *Create All* does:** Create all will automatically create a single set item for every found item listed. Each item will contain one search (the item's text) and one annotation (of type 'Web Search'). If 'Search Context' has a value then context will be added to all of the created annotations.

## Changes

Annotation set changes work on a review system. Any user on xyAnnotations can propose a change to any annotation set however only the set creator or a set moderator can change the status of a change and integrate the requested change into the annotation set.

### Statuses

- **None:** This is the default state and typically means that the set creator and set moderators have not yet seen the change and applied a status to it.
- **Considering:** This means that the change proposal is being considered and is open for discussion from the set's collaborators.
- **Rejected:** The proposal was reviewed by the creator and/or moderator(s) and consequently rejected. A rejected change proposal can still have its status changed to 'Considering' or 'Accepted' anytime after being rejected.
- **Accepted:** The proposal was reviewed by the creator and/or moderator(s) and implemented into the annotation set. This cannot be undone.
- **Deleted:** Technically not a status because the change is completely removed from xyAnnotations, but the set creator and moderators can delete a change should they wish.

## Reader

Xyfir Annotations' reader allows you to import any `EPUB` format ebook and have an annotation set's items inserted into the book's content. This allows you to easily view the annotations without needing to switch to another ebook reader application or device that supports xyAnnotations.

### Editing

The reader also doubles as an editor for the set's items. You can add, edit, and remove items from within the reader itself. Simply click the edit button to toggle the edit mode. Once in edit mode you can click on highlighted text to manage the item that matched the highlighted text. You can also select text while in edit mode and the edit button will change into a 'create' button that when clicked allows you to create an annotation set item from the selected text.

If you are the creator of, or a moderator for the annotation set, any additions, removals, or modifications will be made directly to the annotation set immediately. If you are only a normal user, any edits you make will create a change proposal. The annotation set will remain unchanged until the set creator or a moderator accepts your change. Be careful not to create conflicting changes.