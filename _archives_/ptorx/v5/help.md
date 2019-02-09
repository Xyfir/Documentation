# Terminology

- **Primary Emails**
  - 'Primary' emails (also known as main, normal, or real emails) are your actual email addresses where you normally receive mail. These emails are where mail sent to your 'proxy' email addresses will be redirected to if the mail matches your filters.
- **Proxy Emails**
  - 'Proxy' emails (also known as redirect, or forwarding emails) are email addresses created via Ptorx that typically act as a middleman for your main emails by receiving mail and redirecting allowed mail to your real email address. Proxy emails increase your security and privacy by keeping your real email address hidden away from spammers, hackers, and other people who shouldn't have it. Proxy emails also allow you to gain more control over the emails you receive and how you receive them via Ptorx's 'filter' and 'modifier' systems.
- **Filters**
  - Filters are searches for a specific component of received mail that check if the specified component does or does not contain a user-specified value. Incoming mail is accepted or rejected based on if the mail matches the user's filters and each filter's 'on match' action.
  - **'Reject on Match' (Blacklist) Filters**
    - If the incoming mail matches _any_ 'Reject on Match' filters, the mail will be rejected.
  - **'Accept on Match' (Whitelist) Filters**
    - If the incoming mail matches _all_ 'Accept on Match' filters and _does not_ match any 'Reject on Match' filters, the mail is accepted.
- **Modifiers**
  - Modifiers are used to manipulate incoming mail that has passed all filters before it is redirected to a main email.
- **Regular Expressions**
  - Regular expressions (also known as regex) are used to increase the abilities of your filters and modifiers. See [RegexOne.com](https://regexone.com/) for tutorials and explanations. Do _not_ enable regular expressions if you don't know how to use them.
- **Messages**
  - Messages (also known as mail) are any emails sent, received, or redirected by proxy email addresses.

# Proxy Emails

## Fields

- **Name** (optional, defaults to untitled) - A proxy email's name is there to be used as an extra identifier to help with searches. This is best used when you randomly generate your proxy email addresses.
- **Description** (optional, defaults to creation date) - A proxy email's description serves mostly the same purpose as its name, however it has a higher character limit.
- **Address** (optional) - This is the 'user' portion of a proxy email: `[user]@domain.com`. You can enter in whatever name you want (if it's available and of valid length and characters), or you can leave it blank and Ptorx will automatically generate one for you using a random English word and random numbers.
- **Domain** (optional, defaults to `ptorx.com`) - This is the 'domain' portion of a proxy email: `user@[domain.com]`. Possible values for this are global domains like `ptorx.com` and any custom domains that you have added to your account.
- **Redirect To** - This is your 'real' email address where any mail that is sent to your proxy email will be redirected to. This is the only required field when creating a proxy email _unless_ you choose the 'No Redirect' option, at which point this field's value will be ignored.
- **Spam Filter** (optional, enabled by default) - When enabled, Ptorx will automatically block redirects for any emails that are determined to be spam. Mail marked as spam _cannot_ be viewed in any way unless `Save Mail` is enabled.
- **Save Mail** (optional, disabled by default) - When enabled, Ptorx will temporarily save incoming mail for three days, and certain parts of each message for thirty days. Saving mail is required if you wish to be able to reply to messages sent to your proxy email. If `Spam Filter` is enabled, any messages marked as spam will be saved as type 'spam', alongside the 'rejected' and 'accepted' types. Mail saved to Ptorx can be viewed in the 'Messages' section when viewing a proxy email.
- **No Redirect** (optional, disabled by default) - When enabled, mail will _not_ be redirected to a real address, even if you set a value for `Redirect To`. Incoming mail will be saved to Ptorx for three days and will only be viewable from Ptorx's website or application.
- **Direct Forward** (optional, disabled by default) - When enabled, mail will be forwarded to your primary email directly from the sender's address. This means instead of the sender showing as your Ptorx proxy email, it will be the email address of the person who sent the original message. Due to the way direct forward works, there are certain risks and also limitations on what features you can use. Certain filters, `Save Mail`, `No Redirect`, and all modifiers will be disabled. _WARNING_: You will be able to reply to the original sender from within your preferred email app/site/client, however it will _not_ send from your Ptorx address. Replying to mail sent with `Direct Forward` enabled will cause your primary email to be shown to the person you are replying to!
- **Modifiers** and **Filters** (optional, none linked by default) - You can optionally 'link' modifiers and filters to your proxy email.

## Deleting Proxy Emails

By deleting a proxy email, that specific email is completely removed from your account, however it remains on Ptorx's system simply as a placeholder to prevent that address from ever being claimed again in the future. This means that once you delete a proxy address there is no way for you or another user to claim that address. Deleted proxy emails will of course not redirect or save received mail.

## Replying to Redirected Mail

You can safely reply to mail that is sent to a proxy email if that proxy email has `Save Mail` enabled. You can reply to this mail either from within the Ptorx application by viewing the proxy email and going to the 'Messages' tab, or by simply replying to the message you received in your email client as you would reply to any other message.

There are some limitations with this feature:

- You will only be able to reply to mail from within the Ptorx application for 3 days.
- You will only be able to reply to mail from your email client for 30 days after receiving the original message.
- If you reply to mail from within your email client, that client must support the `Reply-To` email header. Most popular clients (like Gmail) should support this.

The replies will be sent to the original sender using the address of the proxy email that received the message.

## Template

To save time when creating instant or custom proxy emails, you can set one of your existing proxy emails as a _template_, meaning that its settings (except for address) will be used during the creation process: either prefilled into the custom form, or automatically used for the instant creator.

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
  - If you have _Regular Expression_ enabled for the filter, you can only use regular expressions for the header value, not the header name. The value for the header name must be an exact match.

# Modifiers

Modifiers give you expanded control over the content of the mail that gets sent to you. Modifiers are only applied to mail being redirected to a main email. If you have 'Save Mail' enabled on a proxy email, the saved mail will be unmodified.

## Modifier Types

- **Find and Replace**
  - Finds and replaces content in the email's message body.
  - When _Regular Expression_ is enabled, you gain special capabilities in the 'Replace' value via the `$` character.
    - `$$`: If you just want to insert a '$' character, you'll have to use `$$`.
    - `$&`: Inserts the matched substring.
    - `` $` ``: Inserts the portion of the string that precedes the matched substring.
    - `$'`: Inserts the portion of the string that follows the matched substring.
    - `$n`: Where _n_ is a non-negative integer lesser than 100, inserts the nth parenthesized submatch string.
  - When _Regular Expression_ is enabled, a handful of regular expression flags are available to you.
    - `g`: Global match - Find all matches rather than stopping after the first match.
    - `i`: Ignore case - Treat A and a, B and b, C and c, etc as the same characters.
    - `m`: Multiline - Treat beginning and end characters (^ and $) as working over multiple lines (i.e., match the beginning or end of each line (delimited by \n or \r), not only the very beginning or end of the whole input string).
    - `u`: Unicode - Treat pattern as a sequence of unicode code points.
    - Flags are entered all in one string of characters. For example if you wanted all of them: `gimu` or if you just wanted global and multiline flags: `gm`.
- **Subject Overwrite**
  - Completely replaces a message's subject with a value.
- **Subject Tag**
  - Tags a message's subject with a value.
  - If _Prepend Tag_ is enabled, the tag is prepended to the beginning of the subject, otherwise it is appended to the end.
- **Concatenate**
  - Allow you to concatenate (add) two variables within an email together.
  - Example: if your 'Add' variable is 'Sender Address' and your 'To' variable is 'Subject' and you receive an email where the sender's email is 'sender@ptorx.com' and the subject is 'Hello!', your modified email subject will be 'Hello! - sender@ptorx.com'. This assumes you're using the default separator value of ' - ' which can also be modified.
  - If _Prepend_ is enabled, the 'Add' variable and separator are prepended to the 'To' variable instead of appended.
- **Builder**
  - A 'Builder' modifier allows you to build the value of a specified email variable. You can write out the exact value that you want the field to be set as while also using variables from the email itself.
  - Variables are accessed using the `{{variable}}` syntax where `variable` is the name of the variable you wish to insert at that location.
  - In the event that an email does not provide the needed data for a variable, the variable reference will simply be removed from the 'Builder' value.
  - Available variables are:
    - `{{sender}}` - Depending on the email itself, this can be either just the sender's email address (like `{{sender-address}}`) _or_ a value of the following format: `{{sender-name}} <{{sender-address}}>`.
    - `{{subject}}` - The email's subject.
    - `{{body-html}}` - The message body's HTML.
    - `{{body-text}}` - The message body's plain text. In the event that the email contains HTML, this is the same value but with the HTML stripped out.
    - `{{sender-name}}` - The sender's name, taken from the `{{sender}}` variable _if_ the variable contains the sender's name.
    - `{{real-address}}` - The email address that the message will be redirected to.
    - `{{proxy-address}}` - The address of your proxy email that received the message.
    - `{{sender-domain}}` - The domain of the sender's address.
    - `{{sender-address}}` - The email address of the sender.
    - `{{header('Header-Name', 'Default Value')}}` - Allows you to get the value of a specified email header. `Header-Name` should be replaced with the name of the header whose value you wish to insert. If the specified header is not present in an email, `Default Value` be put in its place. Providing a default value is required!

### Global Modifiers

Global modifiers are modifiers available to everyone and do not need to be created, they can simply be linked to any proxy email without any customization needed.

- **Text Only**
  - Only uses the `text/plain` portion of an incoming message. **Beware:** This content may contain unescaped HTML code. However, because it's being sent as text and not as HTML it _should_ just render as text in whatever email client you receive the modified message in.

# Domains

A domain on Ptorx is a normal domain name that can be used to create proxy emails. All Ptorx users get access to the `ptorx.com` domain. Optionally, you can add your own domain to Ptorx, or request access to a domain added by another Ptorx user.

Using your own domain, or those used by few other users, decreases the chance of that domain being blacklisted by websites that want to prevent accounts from being created on their service with what they see as 'throwaway' emails.

## Adding Your Domain to Ptorx

You can add your own domains to Ptorx, and use them to create proxy emails just like you would with a normal `@ptorx.com` proxy email. This is available at no extra cost and you can add as many domains as you like!

After initiating the process of adding your domain to Ptorx, you must verify that you own the domain, so don't bother trying to add a domain you don't own to Ptorx. You will verify ownership by setting a few DNS records:

| Type  | Hostname                              | Value                                              |
| ----- | ------------------------------------- | -------------------------------------------------- |
| TXT   | yourdomain.com                        | v=spf1 include:mailgun.org ~all                    |
| TXT   | `somename`.\_domainkey.yourdomain.com | `k=rsa; p=` (actual name and value given on Ptorx) |
| CNAME | email.yourdomain.com                  | mailgun.org                                        |

and the following MX DNS records:

| Type | Priority | Value           |
| ---- | -------- | --------------- |
| MX   | 10       | mxa.mailgun.org |
| MX   | 10       | mxb.mailgun.org |

`yourdomain.com` should be whatever domain you entered into Ptorx. If you already have MX records set on your domain for another service, you should use a subdomain for mail that Ptorx will handle.

**Note:** Ptorx uses [MailGun](https://www.mailgun.com/)'s infrastructure for handling emails, so your domain needs the previous records to communicate with their servers. Removing these records after verification will prevent your domain from working with Ptorx.

It may take up to a day or two for these records to propagate. Use the 'verify' button when viewing your domain on Ptorx to check the values again.

## Requesting Access to Domains

If another user has already added a domain to Ptorx, you can request access to it by using the 'request access key' that is generated for you when you attempt to add the domain. You must send that key to the user who originally added the domain to Ptorx, and they must then authorize that key to use their domain.

A request key is used to prevent your account's information from needing to be shared with the domain owner.

**Warning!** Using another user's domain can be risky! If they decide to remove their domain from Ptorx, or don't renew its registration before it expires, you will lose access to any proxy emails you have created with that domain.

## Deleting a Domain

Deleting a domain from your account if you are the domain's creator will result in all users of your domain, including you, losing any proxy emails they have created with your domain. Your domain will be completely removed from Ptorx. You will be able to add your domain back to Ptorx in the future should you wish.

Deleting a domain from your account if you are a user of the domain will result in any proxy emails you have with the domain being deleted. You will not be able to regain access to those proxy emails, even if you add the domain back to your account.

Deleting a user from your domain if you are the domain's creator will result in all of the user's proxy emails linked to your domain being deleted. The user will not be able to access your domain again without you authorizing their new request key.

# Credits

Credits are the currency with which actions on Ptorx are paid for. Sending, receiving, redirecting, and replying to mail all cost credits. Credits can be obtained for free by joining the site, through occasional giveaways, and through the referral system. They can also be purchased directly, or obtained for free through various actions like running a cryptocurrency miner within the app.

Whenever your account runs out of credits, all of your proxy emails will be _disabled_, meaning they will no longer receive, redirect, or allow you to send or reply to mail. Once you obtain more credits (even just one!) your proxy emails will be activated and become functional again. Any mail your proxy emails received while disabled will _not_ be accessible in any way as emails received by disabled proxy emails are completely ignored by our system.

## Actions

| Action                       | Cost      |
| ---------------------------- | --------- |
| Send mail                    | 1 credit  |
| Receive mail                 | 1 credit  |
| Redirect (Receive + Send)    | 2 credits |
| Reply from app               | 1 credit  |
| Reply from third-party inbox | 2 credits |

Note that the _Redirect_ action is really just a combination of the _Send_ and _Receive_ actions, as is the _Reply from third-party inbox_ action. To clarify further: if someone sends you an email that is then redirected to one of your primary emails, that will only cost 2 credits.

Everything else on Ptorx is free, however there are certain actions that will require you to have one or more credits in your account, even though they won't actually charge you any to complete the action.
