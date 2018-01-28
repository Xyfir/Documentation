# Terminology

## Critical

- The xyAnnotations annotation system is broken down into repositories called `annotation sets`.
- An `annotation set` is a set of `items` that contain annotations for a specific book.
- A `set item` is further broken down into `searches` and `annotations`. A set item can have multiple searches and multiple annotations.
- A `search` is a collection of `subsearches` (`main subsearch`, `before subsearch`, `after subsearch`) that together point to certain text within a book's content where the item's annotations will be applied.
- An `annotation` is the specific annotation (document, link, image, etc) that will be applied to the book's content matched by the search.

## Useful

- A `change` is either a proposed, accepted, or rejected modification that a user has suggested for an annotation set.
- A `linked item` is an item within an annotation set that gets its data from an item in another annotation set. Any changes made to the source item will be reflected on the linked item.
- A `global annotation set` is an annotation set that is created not to annotate a specific book or set of books, but instead to provide common, high quality items that other annotation sets can link to. Global annotation sets are managed by xyAnnotations moderators, however as always, anyone is free to submit change proposals.

## Visual Hierarchy

- Annotation set
  - Set item
    - Search
      - Main subsearch
      - Before subsearch
      - After subsearch
    - Annotation

## Notes

- `Annotation sets` are typically shortened to just `sets`.
- `Set items` are typically shortened to just `items`.
- In certain contexts `annotations` may be used to refer to all of the items within a single `annotation set`.
- In certain contexts, a `search` may be used to refer to a single `set item` search, or to a `main`, `before`, or `after` subsearch within a single `set item` search.

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

- **Main Subsearch**
  - This is the actual search query that will be searched for and highlighted within the book's content. This is either a normal string of characters or a regular expression if the search is marked as a regular expression.
- **Searches are Regular Expressions**
  - This is a true/false value that tells whether the subsearch queries (main/before/after) are just normal strings or regular expressions.
- **Before Subsearch**
  - This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "before" subsearch does not come *before* the match for the "main" subsearch. This search query should only have a single match within a book's entire content.
  - Imagine the book's content: `... :before-search-match: ... :main-search-match: ...`. If `:before-search-match:` does not come *before* `:main-search-match:`, then `:main-search-match:` will *not* be accepted.
- **After Subsearch**
  - This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "after" subsearch does not come *after* the match for the "main" subsearch. This search query should only have a single match within a book's entire content.
  - Imagine the book's content: `... :main-search-match: ... :after-search-match: ...`. If `:after-search-match:` does not come *after* `:main-search-match:`, then `:main-search-match:` will *not* be accepted.

#### Notes

##### Global and Specific Searches

A search is considered 'global' if there are no before/after subsearch queries. A global search can match anywhere within a book. Inversely, a search that is not 'global' is considered a 'specific' search.

##### Avoid Multiple Paragraphs

You should avoid including more than one paragraph of text into a main/before/after subsearch query. Due to the way different ebook formats handle separating paragraphs, it is very likely that your search will not find any matches. If you want your annotation to highlight multiple paragraphs or parts of multiple paragraphs, you will need to create a separate Search for each.

##### Avoid Specially Formatted Text

You should avoid including portions of specially formatted text within your search if your search is not entirely just the specially formatted text. For example, assume the text below is part of a book you are annotating.

> The quick **brown fox** jumps over the *lazy dog*.

As you can see there are two different sets of specially formatted text within the normally-formatted content. In this instance you could create a search for `jumps over the` or `brown fox`, but you *should not* create a search for `quick brown fox` or `the lazy dog` because when the ebook reader searches through the content there will be hidden special characters that signify the formatted text, and they will break your search.

For example, in an ebook format that uses HTML, that same text might look like:

    The quick <strong>brown fox</strong> jumps over the <em>lazy dog</em>.

Those special characters prevent your search from matching, unless you create a regular expression and account for those hidden characters.

To make things simple, it's recommended to just avoid including specially formatted text in your search *unless* your search contains *only* that specially formatted text. If you need to match the entire thing you should use multiple searches and take advantage of before/after subsearches to ensure you're only matching what you really want.

### Annotations

A set item can contain multiple annotations of the same or different types. Annotations should explain the content that the item's searches match in a way that is relevant to the goal of the annotation set.

There are multiple annotation types:

#### (1) Document Annotation

Documents are Markdown-supported text.

#### (2) Link Annotation

An `HTTP` or `HTTPS` link. Most reader apps will attempt to display the link within the app first, and fallback to or optionally support navigating to the link directly.

#### (3) Web Search Annotation

Not to be confused with an annotation set item's *searches*, this is a web search query to be used with any general search engine (Google, Bing, etc).

##### Context

When a reader views an annotation that has a context value, they can optionally enable the context which will add the 'Context Search' value to the normal web search query. Typically, this is the book title, series, or some other text that can be used to narrow down the scope of possible results to increase relevancy. Imagine you're creating annotations for a historical fiction book and you're creating an annotation for the text 'George Washington'. You might want the search query to default to just his name while allowing an optional search that adds the name of the book title.

#### (4) Image Annotation

A direct `HTTP`/`HTTPS` link to a `PNG`, `JPG`, or `GIF` formatted image. It should be *direct* link, to the image file, and not simply a webpage that loads the image.

If multiple links are provided it becomes an album.

#### (5) Video Annotation

A direct `HTTP`/`HTTPS` link to a `WEBM` or `MP4` video file, or a YouTube or Vimeo *embed* link.

If multiple links are provided it becomes a playlist.

#### (6) Audio Annotation

A *direct* `HTTP`/`HTTPS` link to an audio file, no specific format is required.

If multiple links are provided it becomes a playlist.

#### (7) Map Annotation

Can be a direct link to an *interactive* map or a search query for use in a real-world map (Google Maps, Bing Maps, etc).

If the map is just a static image, use the **Image** annotation type.

#### Linking to External Content

When linking to external content (links, images, videos, audio, maps), be sure that the site you're linking to allows you to do so.

You should also check if the site has any restrictions in place, like a [CSP](https://en.wikipedia.org/wiki/Content_Security_Policy) that will prevent another site from embedding their content, as this can prevent ebook readers from properly rendering the content you linked to.

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

**Generating a Map annotation:** By checking the box for *Map Annotation* our system will automatically create a map annotation using the item's text as a search query (for Google Maps, etc). Enable map annotation generation if your selected item's text is a place that can be found on an actual map (a place, city, zip code, etc).

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

# Formatting Descriptions and Posts

Descriptions and posts on xyAnnotations support [Markdown](https://daringfireball.net/projects/markdown/syntax) which allows you to format your text.

## Reference Syntax

xyAnnotations adds special syntax on top of Markdown that allows you to easily reference things on xyAnnotations like annotation sets, items, users, etc.

- **Users**
  - `u/AutoAnnotator` -> `[u/AutoAnnotator](#/users/AutoAnnotator)`
  - `u/1234` -> `[u/1234](#/users/1234)`
- **Sets**
  - `s/1234` -> `[s/1234](#/sets/1234)`
- **Items**
  - `i/1234` -> `[i/1234](#/item/1234)`
- **Changes**
  - `c/1234` -> `[c/1234](#/change/1234)`