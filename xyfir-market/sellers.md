# Creating a Sales Thread

To create a sales thread, start by submitting a new text post. You will create a thread that only xyMarketBot and other subreddit moderators will be able to see. If you provided all the needed data and your syntax is correct (xyMarketBot will notify you if you did something wrong), then the sales thread you posted will be removed from the subreddit and reposted under xyMarketBot's account. Doing this gives xyMarketBot the ability to update the thread whenever needed. You will receive a link to your reposted thread once your original sales thread is approved.

A sales thread will remain 'active' for one week. After this time the sales thread will be removed from the subreddit and you will be able to use the `repost` command if you wish to repost it.

In the body text of your submission, you'll need to post everything in a specific format which will look like this:

```
@structured

**Field Name** Some value

**Another Field** Another value

**A Multiline Field** Some value

that spans

multiple lines.
```

Be careful with your spaces. Adding a space before `**Field**` or *not* adding one after `**Field**` will cause xyMarketBot to miss the data. Field names and their values (with the exception of `**Category**`'s value) are case-insensitive.

Note that this format is strictly for what xyMarket calls *structured* threads. A structured thread is a sales thread posted in a specific format/syntax that xyMarketBot can read and understand. Structured threads are given access to all of xyMarket's features like autobuy or feedback. Any thread that does not begin with `@structured` (must be the first line!) will be considered an *unstructured* thread.

**Required** fields are:

- `**Type**`
  - This describes what type of item you're selling.
  - Possible values are: `Physical Item`, `Physical Service`, `Digital Item`, `Digital Service`.
- `**Category**`
  - This allows the bot to properly categorize your sales thread in the Daily Thread.
  - The category you choose must match *exactly* (even capitalization) with one in the dropdown list found [here](https://xyfir.com/#/market).
- `**Description**`
  - Describe what you're selling.
  - While multiple lines here are supported, you should try and keep this as short as possible.
  - Do not use the description to post prices or anything that already has a dedicated field.
  - Do not use the description to create your own custom fields using the `**Field Name** <value>` format.
- `**Price**`
  - The price (in USD) of whatever you're selling. This price will be converted to the appropriate currency upon purchase.
  - The price must be like `$X.XX` where `X.XX` is the dollar and cents amount. Cents are required, even if it is just `00`.
  - The minimum price is `$5.00` if `**Tracking** True`.
- `**Currency**`
  - This is a space-delimited list of currencies that you'll accept.
  - You can add any currency of any name here, however xyMarketBot will look for three currencies if you have set `**Tracking** True`: `BTC` (Bitcoin), `LTC` (Litecoin), and `ETH` (Ether/Ethereum). It's recommended that you stick to the same format of all-caps and known shortened names for currencies. For example `PP` for PayPal or `WU` for Western Union.
  - Your currency field might look something like this: `**Currency** BTC PP ETH`.

**Optional** fields are:

- `**Images**`
  - A space-delimited list of direct links to images for whatever you're selling.
  - Must be a direct link to a `png`, `jpg`, or `gif` file.
  - Example: `**Images** http://i.imgur.com/XXXXXXXXXXXXXX.jpg https://i.imgur.com/XXXXXXXXXXXXXZ.png`
- `**NSFW**`
  - When `True`, the sales thread will be marked as NSFW.
  - Use this if some of the content in your post is NSFW but the item being sold fits better in a category other than NSFW.
- `**Tracking`**`
  - When `True`, purchases using supported currency will be tracked by xyMarketBot, meaning the purchase/sale will be verifiable for both the buyer and the seller. Both buyers and sellers will be able to leave positive or negative feedback after the purchase is complete.
  - Supported currencies are `BTC`, `LTC`, and `ETH`.
  - Remember to set `**Addresses**` when this is enabled.
  - There is no fee, and xyMarketBot does not act as a middleman. It simply *tracks* a payment from a buyer to a seller for a specific order.
- `**Escrow**`
  - **NOTE:** Escrow is currently disabled while we determine its feasiblity and consider other possible implementations.
  - When `True`, the buyer can *optionally* decide to make a purchase using escrow.
  - The buyer will pay an added 1% fee.
  - Payment is not released to the seller until the buyer sends the `release escrow` command. The seller can request the escrow to be released at any time to remind the buyer. In the event of a dispute, a subreddit moderator will become involved and choose who gets the payment.
  - `**Tracking**` must be `True`.
- `**Autobuy**`
  - When `True`, the seller will provide 'autobuy items', each of which is a line of text representing whatever the buyer purchased.
  - After the sales thread is approved, the seller can private message commands to xyMarketBot to manage stored autobuy items.
  - After a purchase, the buyer will automatically receive the oldest autobuy item stored, and then that item will be removed from the list.
  - This will automatically add the `**Stock**` field to the reposted sales thread. `**Stock**` will be updated based on the amount of autobuy items currently available.
  - `**Tracking**` must be `True`.
  - `**Type**` must be `Digital Item`.
- `**Addresses**`
  - This field is **required** if `**Tracking** True`.
  - You must provide payment addresses for whichever of the three supported currencies you accept: `BTC`, `LTC`, and/or `ETH`.
  - The value should look like: `BTC=1YourBitcoinAddressObR76b53LETtpyT LTC=3YourLitecoinAddressHXHXEeLygMXoAj ETH=0xYourEthereumAddress32487E1EfdD8729b87445`.
  - Due to the way xyMarket's payment verification system works, it is highly recommended that you use unique addresses for each sales thread, and that you do not use the addresses to receive payments outside of xyMarket.

# Revising a Sales Thread

The only sales threads that can be revised are those which xyMarketBot has found an error in and not yet approved. A sales thread is revised by updating your thread and then commenting on the thread `u/xyMarketBot revise`.

Sales threads can only be revised for up to one hour after originally being posted, after which time they will be automatically removed from the subreddit.

Don't worry about making a mistake because only xyMarketBot will see it and it will let you know where you went wrong.

# Your Thread ID

The id of your sales thread will be used for numerous commands that must be sent to xyMarketBot as a private message. The id of your thread can be found in the URL when viewing your thread. For example in `https://www.reddit.com/r/xyMarket/comments/6ruq2h/daily_thread/`, the thread id is `6ruq2h`.

# Managing Autobuy Items

If you have a sales thread with `**Autobuy** True`, you can add, list, and clear the autobuy items stored on in xyMarketBot.

## Add Items

Add autobuy items for thread to xyMarketBot's database.

**Syntax**
```
add autobuy items to :threadId:

<items here>
```

**Example**
```
add autobuy items to 6u5fft

item 1

item 2

item 3
```

Each autobuy item must occupy a single line and be preceded by an empty line.

## List (Show) Items

List all autobuy items for thread stored in xyMarketBot's database.

**Syntax**
```
list autobuy items in :threadId:
```

**Example**
```
list autobuy items in 6u5fft
```

## Clear Autobuy Items

Delete all saved autobuy items for thread from xyMarketBot's database.

**Syntax**
```
clear autobuy items in :threadId:
```

**Example**
```
clear autobuy items in 6u5fft
```

# Removing a Sales Thread

Comment on your sales thread `u/xyMarketBot remove` *or* `u/xyMarketBot delete`.

Removing a sales thread will remove it from the subreddit and mark it as 'removed' in xyMarketBot's database. Removed threads can be reposted anytime in the future. Autobuy items are left in xyMarketBot's database in case the thread is later reposted.

Constantly removing and reposting your thread to bump it to the top of `/r/xyMarket/new` will get you banned. Only promoted threads can do that.

# Requesting Escrow Funds Release

Send a reminder to the buyer to release funds held in escrow and complete the order.

**Syntax**
```
request escrow for :orderId:

<optional message to buyer here>
```

**Example**
```
request escrow for 54321

You have received the item. Please release funds. Thank you.
```

# Repost Sales Thread

Repost an expired, removed, (or active, if promoted) sales thread.

Comment `u/xyMarketBot repost` on the sales thread.

Autobuy items, feedback, and orders that were previously attached to the old thread will be updated to point to the reposted sales thread.

# Request Verification

Request that a xyMarket moderator verify your sales thread and your offerings. Command must be commented on the thread.

**Syntax**
```
u/xyMarketBot request verification

<your message here>
```

**Example**
```
u/xyMarketBot request verification

PM me for proof.
```

# Promote Thread

## Features
- Is not expired and removed from the subreddit after one week, but instead expires when the promotion period ends.
- Can remove and repost their sales thread whenever they want in order to bump it back to the top of r/xyMarket/new (or for whatever other reason).
- Is added to the top of its category in the stickied daily thread.
- Receives a gem/diamond emoji next to its title in the stickied daily thread to make it stand out.
- Will receive new features that are added for promoted threads in the future.

Promoting a thread currently costs $2.50 (USD) per month.

## Command

Initiates the process of promoting your sales thread.

**On Thread Syntax**
```
promote for :months: month|months using BTC|LTC|ETH
```

**On Thread Example**
```
promote for 1 month using BTC
```

**In Private Message Syntax**
```
promote :threadId: for :months: month|months using BTC|LTC|ETH
```

**In Private Message Example**
```
promote 6u5fft for 2 months using LTC
```