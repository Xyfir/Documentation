# Books

## Searching

By default, just typing in a search query will return any books whose title or author(s) match your search.

### Advanced Searches

You can also use advanced search queries using the following syntax: `field:value "field2:search with spaces" ...`

In this example, xyReader will return any books whose `field` property contains `value`, _AND_ whose `field2` property contains `value with spaces`. As you can see, if your search query contains spaces, you must enclose the entire search within double quotes. The search value cannot contain double quotes.

A better, real-world example would be: `"title:Huckleberry Finn" "creator:Mark Twain"`. This will only return books whose `creator` property contains `Huckleberry Finn` and whose `creator` (always singular) property contains `Mark Twain`.

Available search fields are:

- `id` - _number_
- `title` - _string_
- `creator` - _string_
- `pubdate` - _iso utc date string_
- `publisher` - _string_
- `description` - _string_

## List Views

You can switch your book list view by clicking the button next to the search box when viewing your books. There are three different list views available for you to choose from:

- **Compact**
  - This view was designed to work well on any screen size. Only the most important details and controls are available.
- **Grid**
  - This view was designed to work well on tablets, or other large screens with limited controls. It shows large covers, only the most necessary information, and limited controls.
- **Table**
  - This view was designed only for large screens, and mainly for desktop environments. If your screen is too small, this option will not be available. The table view shows you the most information, gives you the most controls for your books, and also offers extended sorting features that the other views lack.

# Reader

xyReader currently only supports EPUB format ebooks. If you'd like to convert a non-EPUB ebook to EPUB format, try [Calibre](https://calibre-ebook.com).

## Switching Highlight Modes

You can switch highlight modes by tapping the bottom of the reader screen on or near the percent and page status when the overlay is hidden. Simply tap that section of the page and the status will update to tell you the current highlight mode.
