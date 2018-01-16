# Books

xyBooks lets you store and manage ebooks of just about any format and size. Everything is stored in the cloud, which means a change to your library made on one device will sync across all of the other devices you use to access your xyBooks account.

## Searching

By default, just typing in a search query will return any books whose title or author(s) match your search.

### Advanced Searches

You can also use advanced search queries using the following syntax: `field:value "field2:search with spaces" ...`

In this example, xyBooks will return any books whose `field` property contains `value`, *AND* whose `field2` property contains `value with spaces`. As you can see, if your search query contains spaces, you must enclose the entire search within double quotes. The search value cannot contain double quotes.

A better, real-world example would be: `"title:Huckleberry Finn" "authors:Mark Twain"`. This will only return books whose `title` property contains `Huckleberry Finn` and whose `authors` (always plural) property contains `Mark Twain`.

Available search fields are:
- `author_sort` - *string*
- `authors` - *string*
- `id` - *number*
- `rating` - *number*
- `series` - *string*
- `series_index` - *number*
- `tag` - *string*
- `title` - *string*
- `pubdate` - *iso utc date string*
- `publisher` - *string*
- `timestamp` - *iso utc date string*
- `comments` - *string*

## List Views

You can switch your book list view by clicking the button next to the search box when viewing your books. There are three different list views available for you to choose from:
- **Compact**
  - This view was designed to work well on any screen size. Only the most important details and controls are available.
- **Grid**
  - This view was designed to work well on tablets, or other large screens with limited controls. It shows large covers, only the most necessary information, and limited controls.
- **Table**
  - This view was designed only for large screens, and mainly for desktop environments. If your screen is too small, this option will not be available. The table view shows you the most information, gives you the most controls for your books, and also offers extended sorting features that the other views lack.

# Reader

While xyBooks supports just about any ebook format when it comes to storing and managing, our reader currently only supports EPUB format ebooks. If you'd like to convert a non-EPUB ebook to EPUB format, check the 'Convert Format' section of your book's 'Add Format' page.

Once you've opened a book once on your device that books should be downloaded and available for offline-reading. Please note that if you're offline, many features will not be available and certain changes will not be saved.

## Switching Highlight Modes

Currently there are three different highlight modes: `off`, `notes`, and `annotations`. When `off`, as you'd expect, no text within your book will be highlighted. When `notes`, your own notes will be highlighted within the book's content. When `annotations`, annotations from the selected xyAnnotations set will be highlighted.

You can switch highlight modes by tapping the bottom of the reader screen on or near the percent and page status when the overlay is hidden. Simply tap that section of the page and the status will update to tell you the current highlight mode. The modes are cycled in the following order: off -> notes -> annotation set #1 (if available) -> annotation set #n (if available) -> repeat.