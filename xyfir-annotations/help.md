# Terminology

## Critical

* The xyAnnotations annotation system is broken down into repositories called `annotation sets`.
* An `annotation set` is a set of `items` that contain annotations for linked books or other media.
* A `set item` is further broken down into `searches` and `annotations`. A set item can have multiple searches and multiple annotations.
* A `search` is a collection of `subsearches` (`main subsearch`, `before subsearch`, `after subsearch`) that together point to certain text within a book's content where the item's annotations will be applied.
* An `annotation` is the specific annotation (document, link, image, etc) that will be applied to the book's content matched by the search.

## Useful

* A `change` is either a proposed, accepted, or rejected modification that a user has suggested for an annotation set.
* A `linked item` is an item within an annotation set that gets its data from an item in another annotation set. Any changes made to the source item will be reflected on the linked item.
* A `global annotation set` is an annotation set that is created not to annotate a specific book or set of books, but instead to provide common, high quality items that other annotation sets can link to. Global annotation sets are managed by xyAnnotations moderators, however as always, anyone is free to submit change proposals.

## Visual Hierarchy

* Annotation set
  * Set item
    * Search
      * Main subsearch
      * Before subsearch
      * After subsearch
    * Annotation

## Notes

* `Annotation sets` are typically shortened to just `sets`.
* `Set items` are typically shortened to just `items`.
* In certain contexts `annotations` may be used to refer to all of the items within a single `annotation set`.
* In certain contexts, a `search` may be used to refer to a single `set item` search, or to a `main`, `before`, or `after` subsearch within a single `set item` search.

Continue reading for more in-depth descriptions.

# Annotation Sets

An annotation set is a central container for set items, change proposals, and discussions. An annotation set typically has a 'goal' specified in its description that is set by its creator and moderators. This goal should dictate what type of annotations the set contains and should drive the annotation set's evolution in the future into a specific direction. Because a set can serve so many different purposes, it is important that a goal is set early and is explained clearly in the set's description so that potential contributors can know how to help. Ideas for annotation set goals are: explanations of things referenced in the book (people, places, etc); criticism; translations from one language to another; background information and insights; etc.

**Note** While Annotation Sets are mainly used for books, technically they can annotate any type of media. For simplicity, these docs will mainly assume that an annotation set is used to annotate one or more books.

## Moderators

You can make other users moderators to your annotation set to help with the management of your set. Moderators have all of the special powers that you as a creator have but they can lose their powers if removed from the moderator list while you can never be removed as a creator.

### Global Moderator

A 'Global Moderator' is a user that has moderator powers across the entire site and has full power over all annotation sets regardless of whether they were made a moderator or not. A global mod can also delete annotation sets, something that normal moderators and even a set creator cannot do.

## Items

Annotation set items typically encompass a single part of an annotation set. An item should contain searches that are all relevant to each other while the annotations should all be relevant to the item's searches and should accomplish the goal of the annotation set as a whole.

### Searches

A set item's searches point to sections of a book where the item's annotations should be applied. A search can be any portion of text within a book's content.

Searches contain the following values:

* **Main Subsearch**
  * This is the actual search query that will be searched for and highlighted within the book's content. This is either a normal string of characters or a regular expression if the search is marked as a regular expression.
* **Searches are Regular Expressions**
  * This is a true/false value that tells whether the subsearch queries (main/before/after) are just normal strings or regular expressions.
* **Before Subsearch**
  * This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "before" subsearch does not come _before_ the match for the "main" subsearch. This search query should only have a single match within a book's entire content.
  * Imagine the book's content: `... :before-search-match: ... :main-search-match: ...`. If `:before-search-match:` does not come _before_ `:main-search-match:`, then `:main-search-match:` will _not_ be accepted.
* **After Subsearch**
  * This is another string or regex search query that prevents a potential match for the "main" subsearch query from being accepted if the match for the "after" subsearch does not come _after_ the match for the "main" subsearch. This search query should only have a single match within a book's entire content.
  * Imagine the book's content: `... :main-search-match: ... :after-search-match: ...`. If `:after-search-match:` does not come _after_ `:main-search-match:`, then `:main-search-match:` will _not_ be accepted.

#### Notes

##### Global and Specific Searches

A search is considered 'global' if there are no before/after subsearch queries. A global search can match anywhere within a book. Inversely, a search that is not 'global' is considered a 'specific' search.

##### Avoid Multiple Paragraphs

You should avoid including more than one paragraph of text into a main/before/after subsearch query. Due to the way different ebook formats handle separating paragraphs, it is very likely that your search will not find any matches. If you want your annotation to highlight multiple paragraphs or parts of multiple paragraphs, you will need to create a separate Search for each.

##### Avoid Specially Formatted Text

You should avoid including portions of specially formatted text within your search if your search is not entirely just the specially formatted text. For example, assume the text below is part of a book you are annotating.

> The quick **brown fox** jumps over the _lazy dog_.

As you can see there are two different sets of specially formatted text within the normally-formatted content. In this instance you could create a search for `jumps over the` or `brown fox`, but you _should not_ create a search for `quick brown fox` or `the lazy dog` because when the ebook reader searches through the content there will be hidden special characters that signify the formatted text, and they will break your search.

For example, in an ebook format that uses HTML, that same text might look like:

    The quick <strong>brown fox</strong> jumps over the <em>lazy dog</em>.

Those special characters prevent your search from matching, unless you create a regular expression and account for those hidden characters.

To make things simple, it's recommended to just avoid including specially formatted text in your search _unless_ your search contains _only_ that specially formatted text. If you need to match the entire thing you should use multiple searches and take advantage of before/after subsearches to ensure you're only matching what you really want.

### Annotations

A set item can contain multiple annotations of the same or different types. Annotations should explain the content that the item's searches match in a way that is relevant to the goal of the annotation set.

There are multiple annotation types:

#### (1) Document Annotation

Documents are Markdown-supported text.

#### (2) Link Annotation

An `HTTP` or `HTTPS` link. Most reader apps will attempt to display the link within the app first, and fallback to or optionally support navigating to the link directly.

#### (3) Web Search Annotation

Not to be confused with an annotation set item's _searches_, this is a web search query to be used with any general search engine (Google, Bing, etc).

##### Context

When a reader views an annotation that has a context value, they can optionally enable the context which will add the 'Context Search' value to the normal web search query. Typically, this is the book title, series, or some other text that can be used to narrow down the scope of possible results to increase relevancy. Imagine you're creating annotations for a historical fiction book and you're creating an annotation for the text 'George Washington'. You might want the search query to default to just his name while allowing an optional search that adds the name of the book title.

#### (4) Image Annotation

A direct `HTTP`/`HTTPS` link to a `PNG`, `JPG`, or `GIF` formatted image. It should be _direct_ link, to the image file, and not simply a webpage that loads the image.

If multiple links are provided it becomes an album.

#### (5) Video Annotation

A video on YouTube or Vimeo. You can provide either the normal view link to the video, or the embed link to the video.

If multiple videos are provided it becomes a playlist.

You cannot mix videos from different sources within a single annotation.

#### (6) Audio Annotation

A song or other audio track on SoundCloud. This must be the normal view link (`https://soundcloud.com/:author/:track`).

If multiple links are provided it becomes a playlist.

#### (7) Map Annotation

Can be a direct link to an _interactive_ map or a search query for use in a real-world map (Google Maps, Bing Maps, etc).

If the map is just a static image, use the **Image** annotation type.

#### Linking to External Content

When linking to external content (links, images, videos, audio, maps), be sure that the site you're linking to allows you to do so.

You should also check if the site has any restrictions in place, like a [CSP](https://en.wikipedia.org/wiki/Content_Security_Policy) that will prevent another site from embedding their content, as this can prevent ebook readers from properly rendering the content you linked to.

### Generating Annotations

xyAnnotations has a built-in feature for easily generating simple searches and annotations by using a book's text content. The generator is built to parse the book's content and pull out proper nouns (a specific person, place, or thing). This works for the most part because in English (and some other languages) proper nouns are capitalized in the middle of sentences while nothing else is. By exploiting this rule, xyAnnotations can usually get most of the things that might be interesting and worthy of creating basic 'Web Search' annotations for. This system is definitely not perfect, but if you're looking to quickly pull proper nouns out of a book and create basic set items for them then this feature should be satisfactory.

If you populate an annotation set strictly with items from this feature and you make little to no changes to the generated items it is recommended to title your set 'Generated Annotations' or at least note in the description that all the items were automatically generated so that other users know what to expect.

A fully automated solution is available if you wish to automatically generate annotations for an entire library of ebooks. See [auto-annotator](https://github.com/Xyfir/auto-annotator).

## Changes

Annotation set changes work on a review system. Any user on xyAnnotations can propose a change to any annotation set however only the set creator or a set moderator can change the status of a change and integrate the requested change into the annotation set.

### Statuses

* **None:** This is the default state and typically means that the set creator and set moderators have not yet seen the change and applied a status to it.
* **Considering:** This means that the change proposal is being considered and is open for discussion from the set's collaborators.
* **Rejected:** The proposal was reviewed by the creator and/or moderator(s) and consequently rejected. A rejected change proposal can still have its status changed to 'Considering' or 'Accepted' anytime after being rejected.
* **Accepted:** The proposal was reviewed by the creator and/or moderator(s) and implemented into the annotation set. This cannot be undone.
* **Deleted:** Technically not a status because the change is completely removed from xyAnnotations, but the set creator and moderators can delete a change should they wish.

## Reader

Xyfir Annotations' reader allows you to import any `EPUB` format ebook and have an annotation set's items inserted into the book's content. This allows you to easily view the annotations without needing to switch to another ebook reader application or device that supports xyAnnotations.

### Editing

The reader also doubles as an editor for the set's items. You can add, edit, and remove items from within the reader itself. Simply click the edit button to toggle the edit mode. Once in edit mode you can click on highlighted text to manage the item that matched the highlighted text. You can also select text while in edit mode and the edit button will change into a 'create' button that when clicked allows you to create an annotation set item from the selected text.

If you are the creator of, or a moderator for the annotation set, any additions, removals, or modifications will be made directly to the annotation set immediately. If you are only a normal user, any edits you make will create a change proposal. The annotation set will remain unchanged until the set creator or a moderator accepts your change.

## Public & Unlisted

Marking your annotation set as `Public` means that other users will be able to find it through searches and other means, without needing the id of the annotation set to access it directly. If your annotation is `Unlisted`, it cannot easily be found, however anyone who knows its id can still access it. Not only can you view it directly, but you can also search for it within xyAnnotations or applications that support xyAnnotations using the following syntax: `s/1234`, where `1234` is the id of your set.

# Formatting Descriptions and Posts

Descriptions and posts on xyAnnotations support [Markdown](https://daringfireball.net/projects/markdown/syntax) which allows you to format your text.

## Reference Syntax

xyAnnotations adds special syntax on top of Markdown that allows you to easily reference things on xyAnnotations like annotation sets, items, users, etc.

If you reference a user within a comment, it is considered a 'mention' and that user will be notified that you mentioned them. You can delete your comment to remove the notification from their account that you mentioned them. If you wish to avoid mentioning someone, you should directly link to their account, using `[u/AutoAnnotator](#/u/AutoAnnotator)` instead of `u/AutoAnnotator`.

A reference will only be converted to a link if the character _before_ the reference is a space or the beginning of a line.

* **Users**: `u/AutoAnnotator` or `u/1234`
* **Sets**: `s/1234`
* **Items**: `i/5678`
* **Discussions**: `d/1234`
* **Forums**: `f/5678`
* **Changes**: `ch/1234`
* **Comments**: `co/5678`
* **Books**: `b/1234`
