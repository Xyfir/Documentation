# Terminology

- **Main / Real Emails**
  - 'Main' email (also known as normal or real emails) are your actual email addresses where you normally receive mail. These emails are where mail sent to your 'proxy' email addresses will be redirected to if the mail matches your filters.
- **Proxy Emails**
  - 'Proxy' emails (also known as redirect emails) are email addresses created via Ptorx that typically act as a middleman for your main emails by receiving mail and redirecting allowed mail to your real email address. Proxy emails increase your security and privacy by keeping your real email address hidden away from spammers, hackers, and other people who shouldn't have it. Proxy emails also allow you to gain more control over the emails you receive and how you receive them via Ptorx's 'filter' and 'modifier' systems.
- **Filters**
  - Filters are searches for a specific component of received mail that check if the specified component does or does not contain a user-specified value. Incoming mail is accepted or rejected based on if the mail matches the user's filters and each filter's 'on match' action.
  - **'Reject on Match' Filters**
    - If the incoming mail matches *any* 'Reject on Match' filters, the mail will be rejected.
  - **'Accept on Match' Filters**
    - If the incoming mail matches *all* 'Accept on Match' filters and *does not* match any 'Reject on Match' filters, the mail is accepted.
- **Modifiers**
  - Modifiers are used to manipulate incoming mail that has passed all filters before it is redirected to a main email. 
- **Regular Expressions**
  - Regular expressions (also known as regex) are used to increase the abilities of your filters and modifiers. See [RegexOne.com](https://regexone.com/) for tutorials and explanations. Do *not* enable 'Use Regular Expressions' if you don't know how to use them.

# Filters
Filters can be used to whitelist or blacklist what type of incoming mail is allowed to be redirected to your main address.

## Filter Types
There are multiple filter types that allow you to filter by different components of incoming mail.

- **Subject**
  - Filters by the subject of an incoming email message.
- **Address**
  - Filters by the email address of the sender of an incoming email message. For example: `user@website.com`
- **Domain**
  - Filters by the domain of the email address of the sender of an incoming email message. For example in `user@website.com`: `website.com` would be the domain.
- **Text Content**
  - Filters by the text content of an incoming email message. If the message has any HTML, it will be stripped out before running this filter.
- **HTML Content**
  - Filters by the HTML content of an incoming email message.
- **Header**
  - All emails sent and received on the internet use [headers](http://whatismyipaddress.com/email-header) to store data about the piece of mail. Header filters have two parts: the name of the header, and the value for that header.
  - If you have *Use Regular Expressions* enabled for the filter, you can only use regular expressions for the header value, not the header name. The value for the header name must be an exact match.

# Modifiers
Modifiers give you expanded control over the content of the mail that gets sent to you. Modifiers are only applied to mail being redirected to a main email. If you have 'Save Mail' enabled on a proxy email, the saved mail will be unmodified.

## Modifier Types

- **Encrypt**
  - Encrypt modifiers allow you to encrypt the content of received mail via AES-256 with a user-provided encryption key. Since the mail is redirected, you must have the ability to decrypt the content wherever the mail is received.
- **Find and Replace**
  - Finds and replaces content in the email's message body.
  - When *Use Regular Expressions* is enabled, you gain special capabilities in the 'Replace' value via the `$` character.
    - `$$`: If you just want to insert a '$' character, you'll have to use `$$`.
    - `$&`: Inserts the matched substring.
    - ``$` ``: Inserts the portion of the string that precedes the matched substring.
    - `$'`: Inserts the portion of the string that follows the matched substring.
    - `$n`: Where *n* is a non-negative integer lesser than 100, inserts the nth parenthesized submatch string.
  - When *Use Regular Expressions* is enabled, a handful of regular expression flags are available to you.
    - `g`: Global match - Find all matches rather than stopping after the first match.
    - `i`: Ignore case - Treat A and a, B and b, C and c, etc as the same characters.
    - `m`: Multiline - Treat beginning and end characters (^ and $) as working over multiple lines (i.e., match the beginning or end of each line (delimited by \n or \r), not only the very beginning or end of the whole input string).
    - `u`: Unicode - Treat pattern as a sequence of unicode code points.
    - Flags are entered all in one string of characters. For example if you wanted all of them: `gimu` or if you just wanted global and multiline flags: `gm`.
- **Subject Overwrite**
  - Completely replaces a message's subject with a value.
-  **Subject Tag**
  - Tags a message's subject with a value.
  - If *Prepend Tag* is enabled, the tag is prepended to the beginning of the subject, otherwise it is appended to the end.
- **Concatenate**
  - Allow you to concatenate (add) two variables within an email together.
  - Example: if your 'Add' variable is 'Sender Address' and your 'To' variable is 'Subject' and you receive an email where the sender's email is 'sender@ptorx.com' and the subject is 'Hello!', your modified email subject will be 'Hello! - sender@ptorx.com'. This assumes you're using the default separator value of ' - ' which can also be modified.

### Global Modifiers

Global modifiers are modifiers available to everyone and do not need to be created, they can simply be linked to any proxy email without any customization needed.

- **Text Only**
  - Strips out all HTML and leaves only the text content.
- **Asana**
  - A modifier for users who utilize Asana's email service and want to see the sender's address in the subject of the mail they receive.
  - Example: if you receive an email where the sender's email is 'sender@ptorx.com' and the subject is 'Hello!', your modified email subject will be 'Hello! - sender@ptorx.com'.