# Terminology

- **Primary Emails**
  - 'Primary' emails (also known as main, normal, or real emails) are your actual email addresses where you normally receive mail. These emails are where mail sent to your 'proxy' email addresses will be redirected to if the mail matches your filters.
- **Proxy Emails**
  - 'Proxy' emails (also known as redirect, or forwarding emails) are email addresses created via Ptorx that typically act as a middleman for your main emails by receiving mail and redirecting allowed mail to your real email address. Proxy emails increase your security and privacy by keeping your real email address hidden away from spammers, hackers, and other people who shouldn't have it. Proxy emails also allow you to gain more control over the emails you receive and how you receive them via Ptorx's 'filter' and 'modifier' systems.
- **Filters**
  - Filters are searches for a specific component of received mail that check if the specified component does or does not contain a user-specified value. Incoming mail is accepted or rejected based on if the mail matches the user's filters and each filter's 'on match' action.
  - **'Reject on Match' Filters**
    - If the incoming mail matches *any* 'Reject on Match' filters, the mail will be rejected.
  - **'Accept on Match' Filters**
    - If the incoming mail matches *all* 'Accept on Match' filters and *does not* match any 'Reject on Match' filters, the mail is accepted.
- **Modifiers**
  - Modifiers are used to manipulate incoming mail that has passed all filters before it is redirected to a main email. 
- **Regular Expressions**
  - Regular expressions (also known as regex) are used to increase the abilities of your filters and modifiers. See [RegexOne.com](https://regexone.com/) for tutorials and explanations. Do *not* enable regular expressions if you don't know how to use them.
- **Messages**
  - Messages (also known as mail) are any emails sent, received, or redirected by proxy email addresses.

# Proxy Emails

## Fields

- **Name** (optional, defaults to untitled) - A proxy email's name is there to be used as an extra identifier to help with searches. This is best used when you randomly generate your proxy email addresses.
- **Description** (optional, defaults to creation date) - A proxy email's description serves mostly the same purpose as its name, however it has a higher character limit.
- **Address** (optional) - This is the 'user' portion of a proxy email: `[user]@domain.com`. You can enter in whatever name you want (if it's available and of valid length and characters), or you can leave it blank and Ptorx will automatically generate one for you using a random English word and random numbers.
- **Domain** (optional, defaults to `ptorx.com`) - This is the 'domain' portion of a proxy email: `user@[domain.com]`. Possible values for this are global domains like `ptorx.com` and any custom domains that you have added to your account.
- **Redirect To** - This is your 'real' email address where any mail that is sent to your proxy email will be redirected to. This is the only required field when creating a proxy email *unless* you choose the 'No Redirect' option, at which point this field's value will be ignored.
- **Spam Filter** (optional, enabled by default) - When enabled, Ptorx will automatically block redirects or saves for any emails that are determined to be spam. Mail marked as spam *cannot* be viewed in any way.
- **Save Mail** (optional, disabled by default) - When enabled, Ptorx will temporarily save incoming mail for three days, and certain parts of each message for thirty days. If **Spam Filter** is enabled, any messages marked as spam will not be saved. Mail saved to Ptorx can be viewed in the 'Messages' section when viewing a proxy email. Saving mail is required if you wish to be able to reply to messages sent to your proxy email.
- **No Redirect** (optional, disabled by default) - When enabled, mail will *not* be redirected to a real address, even if you set a value for **Redirect To**. Incoming mail will be saved to Ptorx for three days and will only be viewable from Ptorx's website or application.
- **Direct Forward** (optional, disabled by default) - When enabled, mail will be forwarded to your real email directly from the sender's address. This means instead of the sender showing as your Ptorx proxy email, it will be the email address of the person who sent the original message. Due to the way direct forward works, there are limitations on what features you can use. Certain filters, `Save Mail`, `No Redirect`, and all modifiers will be disabled. WARNING: You will be able to reply to the original sender from within your preferred email app/site/client, however it will *not* send from your Ptorx address. Replying to mail sent with `Direct Forward` enabled will cause your primary email to be shown to the person you are replying to.
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
  - If you have *Regular Expression* enabled for the filter, you can only use regular expressions for the header value, not the header name. The value for the header name must be an exact match.

# Modifiers

Modifiers give you expanded control over the content of the mail that gets sent to you. Modifiers are only applied to mail being redirected to a main email. If you have 'Save Mail' enabled on a proxy email, the saved mail will be unmodified.

## Modifier Types

- **Find and Replace**
  - Finds and replaces content in the email's message body.
  - When *Regular Expression* is enabled, you gain special capabilities in the 'Replace' value via the `$` character.
    - `$$`: If you just want to insert a '$' character, you'll have to use `$$`.
    - `$&`: Inserts the matched substring.
    - ``$` ``: Inserts the portion of the string that precedes the matched substring.
    - `$'`: Inserts the portion of the string that follows the matched substring.
    - `$n`: Where *n* is a non-negative integer lesser than 100, inserts the nth parenthesized submatch string.
  - When *Regular Expression* is enabled, a handful of regular expression flags are available to you.
    - `g`: Global match - Find all matches rather than stopping after the first match.
    - `i`: Ignore case - Treat A and a, B and b, C and c, etc as the same characters.
    - `m`: Multiline - Treat beginning and end characters (^ and $) as working over multiple lines (i.e., match the beginning or end of each line (delimited by \n or \r), not only the very beginning or end of the whole input string).
    - `u`: Unicode - Treat pattern as a sequence of unicode code points.
    - Flags are entered all in one string of characters. For example if you wanted all of them: `gimu` or if you just wanted global and multiline flags: `gm`.
- **Subject Overwrite**
  - Completely replaces a message's subject with a value.
- **Subject Tag**
  - Tags a message's subject with a value.
  - If *Prepend Tag* is enabled, the tag is prepended to the beginning of the subject, otherwise it is appended to the end.
- **Concatenate**
  - Allow you to concatenate (add) two variables within an email together.
  - Example: if your 'Add' variable is 'Sender Address' and your 'To' variable is 'Subject' and you receive an email where the sender's email is 'sender@ptorx.com' and the subject is 'Hello!', your modified email subject will be 'Hello! - sender@ptorx.com'. This assumes you're using the default separator value of ' - ' which can also be modified.
  - If *Prepend* is enabled, the 'Add' variable and separator are prepended to the 'To' variable instead of appended.
- **Builder**
  - A 'Builder' modifier allows you to build the value of a specified email variable. You can write out the exact value that you want the field to be set as while also using variables from the email itself.
  - Variables are accessed using the `::variable::` syntax where `varable` is the name of the variable you wish to insert at that location.
  - In the event that an email does not provide the needed data for a variable, the variable referrence will simply be removed from the 'Builder' value.
  - Available variables are:
    - `::sender::` - Depending on the email itself, this can be either just the sender's email address (like `::sender-address::`) *or* a value of the following format: `::sender-name:: <::sender-address::>`.
    - `::subject::` - The email's subject.
    - `::body-html::` - The message body's HTML.
    - `::body-text::` - The message body's plain text. In the event that the email contains HTML, this is the same value but with the HTML stripped out.
    - `::sender-name::` - The sender's name, taken from the `::sender::` variable *if* the variable contains the sender's name.
    - `::real-address::` - The email address that the message will be redirected to.
    - `::proxy-address::` - The address of your proxy email that received the message.
    - `::sender-domain::` - The domain of the sender's address.
    - `::sender-address::` - The email address of the sender.

### Global Modifiers

Global modifiers are modifiers available to everyone and do not need to be created, they can simply be linked to any proxy email without any customization needed.

- **Text Only**
  - Only uses the text/plain portion of an incoming message. **Beware:** This content may contain unescaped HTML code. However, because it's being sent as text and not as HTML it *should* just render as text in whatever email client you receive the modified message in.

# Domains

A domain on Ptorx is a normal domain name that can be used to create proxy emails. All Ptorx users get access to the `ptorx.com` domain. Optionally, you can add your own domain to Ptorx, or request access to a domain added by another Ptorx user.

Using your own domain, or those used by few other users, decreases the chance of that domain being blacklisted by websites that want to prevent accounts from being created on their service with what they see as 'throwaway' emails.

## Adding Your Domain to Ptorx

You can add your own domains to Ptorx, and use them to create proxy emails just like you would with a normal `@ptorx.com` proxy email. This is available at no extra cost and you can add as many domains as you like!

After initiating the process of adding your domain to Ptorx, you must verify that you own the domain, so don't bother trying to add a domain you don't own to Ptorx. You will verify ownership by setting a few DNS records:

Type|Hostname|Value
---|---|---
TXT|yourdomain.com|v=spf1 include:mailgun.org ~all
TXT|krs._domainkey.yourdomain.com|k=rsa; p=... (full value given on Ptorx)
CNAME|email.yourdomain.com|mailgun.org

and the following MX DNS records:

Type|Priority|Value
---|---|---
MX|10|mxa.mailgun.org
MX|10|mxb.mailgun.org

`yourdomain.com` should be whatever domain you entered into Ptorx. If you already have MX records set on your domain for another service, you should use a subdomain for mail that Ptorx will handle.

**Note:** Ptorx uses [MailGun](https://www.mailgun.com/)'s infrastructure for handling emails, so your domain needs the previous records to communicate with their servers. Removing these records after verification will prevent your domain from working with Ptorx.

If may take up to a day or two for these records to propagate. Use the 'verify' button when viewing your domain on Ptorx to check the values again.

## Requesting Access to Domains

If another user has already added a domain to Ptorx, you can request access to it by using the 'request access key' that is generated for you when you attempt to add the domain. You must send that key to the user who originally added the domain to Ptorx, and they must then authorize that key to use their domain.

A request key is used to prevent your account's information from needing to be shared with the domain owner.

**Warning!** Using another user's domain can be risky! If they decide to remove their domain from Ptorx, or don't renew its registration before it expires, you will lose access to any proxy emails you have created with that domain.

## Deleting a Domain

Deleting a domain from your account if you are the domain's creator will result in all users of your domain, including you, losing any proxy emails they have created with your domain. Your domain will be completely removed from Ptorx. You will be able to add your domain back to Ptorx in the future should you wish.

Deleting a domain from your account if you are a user of the domain will result in any proxy emails you have with the domain being deleted. You will not be able to regain access to those proxy emails, even if you add the domain back to your account.

Deleting a user from your domain if you are the domain's creator will result in all of the user's proxy emails linked to your domain being deleted. The user will not be able to access your domain again without you authorizing their new request key.

# Free Trial

All new users are given a free, two week trial.

To prevent abuse to our system, some restrictions are placed on free trial users:
- Cannot create more than 5 proxy emails a day
- Cannot create more than 15 proxy emails total
- Must complete a verification captcha before creating a proxy email
- Cannot have more than one primary email address
- Cannot send messages or reply to received mail
- Cannot add or request access to custom domains

These restrictions can be removed by purchasing a subscription.