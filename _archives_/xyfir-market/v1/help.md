The bulk of all interaction with this subreddit and the sales threads within it will be made by sending commands or doing actions that the bot account [u/xyMarketBot](https://www.reddit.com/user/xyMarketBot) will listen for and act on.

xyMarketBot in most cases acts on a one-minute loop, where every one minute it checks for new commands and actions to respond to. xyMarketBot will most likely not respond immediately to your action, but if you don't receive a response within a little over a minute something probably went wrong. Almost always xyMarketBot will notify you if something went wrong, however there are a few instances where it will not respond to invalid commands or actions.

You should avoid attempting to send commands to expired or removed threads except when it is explicitly stated that a command will be accepted (for instance the `repost` command).

# r/xyMarketStats User Thread

Every user on xyMarket has at least one thread on r/xyMarketStats. This thread tracks their statistics and other useful data that is helpful in determing a user's trustworthiness.

## Verified Profiles

**Verified Profiles** contains links to a user's profiles that are located outside of xyMarket. The user has proved to xyMarket moderators that they own those profiles.

## Completed Orders

**Completed Orders** displays the amount of completed sales and buys that a user has made that were tracked by xyMarket. This means that these orders went through some system that allows xyMarket to verify that a payment actually took place between two users. Typically this means a BTC, LTC, or ETH payment that went through xyMarketBot.

## Completed Trades

**Completed Trades** displays the amount of trades between two users that have been confirmed on xyMarket. The confirmed trades count is not as trustworthy as the completed orders count, because a trade simply requires two users to send `confirm trade` commands. There is no requirement for an actual payment to take place that xyMarket can track. It is much easier for a malicious user to fake *trades* to boost their reputation than it is for them to fake *orders*.

## Reputation

**Reputation** is a number that is calculated based on the user's xyMarket activity. Mainly it is dependent on feedback received for confirmed orders and the amount of confirmed trades the user has. The algorithm for generating this number will likely change in the future.

# Finding a Thread's ID

The id of sales threads are used in numerous commands that must be sent to xyMarketBot as a private message. The id of a sales thread can be found in the URL when viewing the thread. For example in the URL `https://www.reddit.com/r/xyMarket/comments/6ruq2h/some_thread_title/`, the thread id is `6ruq2h`.

# Viewing a Structured Sales Thread

- **Price** - The price of the item being sold in USD. Why USD? It's generally stable, widely used, and easily converted to fiat or cryptocurrencies. USD is always used as the fixed price point, but not always accepted directly by the seller.
- **Sold By** - The user who is selling the item, followed by a link to their [r/xyMarketStats](https://www.reddit.com/r/xyMarketStats) thread.
- **Description** - The seller's description of what they're selling.
- **Type** - The type of item being sold. Has four possible values: `Digital Item`, `Digital Service`, `Physical Item`, `Physical Service`.
- **Category** - The category for the item being sold.
- **Currency** - A list of currencies the seller is accepting.
- **Stock** - An optional field that is present if the seller has **Autobuy** enabled. This field will show the number of autobuy items currently available in xyMarketBot's inventory. You can still purchase from an autobuy thread if the stock is less than the quantity you purchase, however the seller will have to manually send you the items you're owed.
- **Tracking** - If `Yes`, you can optionally decide to make a 'tracked' purchase using BTC, LTC, or ETH. This means that xyMarketBot generates an order, and then tracks the payment for the order to the seller's address. You will be able to give and receive feedback once the order is complete. Making trackable purchases allows you to build your reputation faster than making trades and also acts as a deterrent for bad sellers because they know they'll receive negative feedback if they do anything wrong. Tracked purchases do not cost extra.
- **Verified** - If this value is `Yes`, it means that the seller has somehow proven to a xyMarket moderator that they do indeed have what they are selling. This should add an extra level of assurance for potential buyers. However you should still deal with caution. Moderators, like anyone else, can be fooled.
- **Autobuy** - If `Yes` and you choose to make a purchase using BTC, LTC, or ETH, the items you buy will automatically be sent to you after purchase without having to wait for the seller to manually send you whatever you purchased. This only works for `Digital Item` type sales threads. See **Stock**.
- **Escrow** - *Escrow is currently disabled.*
- **Purchase Links** - You can use the purchase links to easily build the purchase command to send to xyMarketBot. Simply click the link you want and then update the quantity from the default `1` to however many items you wish to buy.

# Making a Tracked Purchase

You can purchase an item publicly by commenting the `purchase` command on the thread itself, or you can do it privately by sending xyMarketBot a private message with the (slightly modified) `purchase` command. There is no difference in the outcome other than one is public and one is private. Use the thread's **Purchase Links** to easily build the purchase command.

Purchases should only go through xyMarketBot if multiple conditions are met: the sales thread is structured and has `**Tracking** True`; you want to make a purchase with BTC, LTC, or ETH; and the sales thread accepts the currency you want to make a purchase with. If any of those are not the case, you must contact the seller directly to make a purchase.

## Command

**On Thread Syntax**
```
u/xyMarketBot purchase :quantity: using BTC|LTC|ETH[ and escrow]
```

**On Thread Example:**
```
purchase 1 using ETH and escrow
```

**In Private Message Syntax**
```
purchase :quantity: of :threadId: using BTC|LTC|ETH[ and escrow]
```

**In Private Message Example**
```
purchase 2 of 6s1psl using BTC
```

## Payment

You will then receive an address to send your payment to and an amount in the chosen currency to send. If you send exactly the specified amount within 15 minutes the order will be considered paid, otherwise it will be expire.

## Autobuy

If the sales thread has Autobuy enabled, you will receive your item immediately after xyMarketBot receives notice of your order being paid and the transaction sufficiently confirmed.

# Posting a Structured Sales Thread

While you can write out the sturctured sales thread yourself, it is highly recommended you use [this form](https://www.xyfir.com/market).

**Required** fields are:

- `**Type**`
  - This describes what type of item you're selling.
- `**Category**`
  - This allows the bot to properly categorize your sales thread in the Daily Thread.
  - The category you choose must match *exactly* (even capitalization) with one of xyMarket's accepted categories.
- `**Description**`
  - Describe what you're selling.
  - While multiple lines here are supported, you should try and keep this as short as possible.
  - Do not use the description to post prices or anything that already has a dedicated field.
  - Do not use the description to create your own custom fields using the `**Field Name** <value>` format.
- `**Price**`
  - The price (in USD) of whatever you're selling. This price will be converted to the appropriate currency upon purchase.
  - The price must be like `$X.XX` where `X.XX` is the dollar and cents amount. Cents are required, even if it is just `00`.
- `**Currency**`
  - This is a space-delimited list of currencies that you'll accept.
  - You can add any currency of any name here, however xyMarketBot will look for three currencies if you have set `**Tracking** True`: `BTC` (Bitcoin), `LTC` (Litecoin), and `ETH` (Ether/Ethereum). It's recommended that you stick to the same format of all-caps and known shortened names for currencies.
  - Your currency field might look something like this: `**Currency** BTC PAYPAL ETH`.

**Optional** fields are:

- `**Images**`
  - A space-delimited list of direct links to images for whatever you're selling.
  - Must be a direct link to a `png`, `jpg`, or `gif` file.
  - Example: `**Images** http://i.imgur.com/XXXXXXXXXXXXXX.jpg https://i.imgur.com/XXXXXXXXXXXXXZ.png`
- `**NSFW**`
  - When `True`, the sales thread will be marked as NSFW.
  - Use this if some of the content in your post is NSFW but the item being sold fits better in a category other than NSFW.
- `**Tracking`**`
  - When `True`, purchases using supported currency will be tracked by xyMarketBot, meaning the completed order will be verifiable for both the buyer and the seller. Both the buyer and seller will be able to leave positive or negative feedback after the purchase is complete.
  - Supported currencies are `BTC`, `LTC`, and `ETH`.
  - Remember to set `**Addresses**` when this is enabled.
  - There is no fee as xyMarketBot does not act as a middleman. It simply *tracks* a payment from a buyer to a seller for a specific order.
- `**Escrow**`
  - *Escrow is currently disabled while we determine its feasiblity and consider other possible implementations.*
- `**Autobuy**`
  - When `True`, the seller will provide 'autobuy items', each of which is a line of text representing whatever the buyer purchased.
  - After the sales thread is approved, the seller can private message commands to xyMarketBot to manage stored autobuy items.
  - After a purchase, the buyer will automatically receive the oldest autobuy item stored, and then that item will be removed from the list.
  - This will automatically add the `**Stock**` field to the reposted sales thread. `**Stock**` will be updated based on the amount of autobuy items currently available.
  - `**Tracking**` must be `True`.
  - `**Type**` must be `Digital Item`.
- `**Repost**`
  - When `True`, the final structured thread will be automatically reposted to other general marketplace subreddits.
- `**Addresses**`
  - This field is **required** if `**Tracking** True`.
  - You must provide payment addresses for whichever of the three supported currencies you accept: `BTC`, `LTC`, and/or `ETH`.
  - The value should look like: `BTC=1YourBitcoinAddressObR76b53LETtpyT LTC=3YourLitecoinAddressHXHXEeLygMXoAj ETH=0xYourEthereumAddress32487E1EfdD8729b87445`.
  - Due to the way xyMarket's payment verification system works, it is highly recommended that you use unique addresses for each sales thread, and that you do not use the addresses to receive payments outside of xyMarket.

# Revising a Structured Sales Thread

The only sales threads that can be revised are those which xyMarketBot has found an error in and not yet approved. A sales thread is revised by updating your thread and then commenting on the thread `u/xyMarketBot revise`.

Sales threads can only be revised for up to one hour after originally being posted, after which time they will be automatically removed from the subreddit.

Don't worry about making a mistake because only xyMarketBot will see it and it will let you know where you went wrong.

# Managing Your Sales Thread's Autobuy Items

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

# Removing Your Sales Thread

**On Thread**
```
u/xyMarketBot remove
```

**In Private Message**
```
remove 6s1psl

// Syntax: `remove :threadId:`
```

Removing a sales thread will remove it from the subreddit and mark it as 'removed' in xyMarketBot's database. Removed structured threads can be reposted anytime in the future. Autobuy items are left in xyMarketBot's database in case the thread is later reposted.

Constantly removing and reposting your thread to bump it to the top of `/r/xyMarket/new` will get you banned. Only promoted threads can do that.

# Reposting Your Sales Thread

Repost an expired, removed, (or active, if promoted) sales thread.

**On Thread**
```
u/xyMarketBot repost
```

**In Private Message**
```
repost 6s1psl

// Syntax: `repost :threadId:`
```

Autobuy items, feedback, and orders that were previously attached to the old thread will be updated to point to the reposted sales thread.

# Verify Your Sales Thread

Request that a xyMarket moderator verify your sales thread and your offerings. Command must be privately sent to xyMarketBot.

**Syntax**
```
request verification for :threadId:

<your message here>
```

**Example**
```
request verification for 6s1psl

Here's proof: ...
```

# Promote Your Sales Thread

## Features
- Is not expired and removed from the subreddit after one week, but instead expires when the promotion period ends.
- Can remove and repost their sales thread whenever they want in order to bump it back to the top of r/xyMarket/new (or for whatever other reason).
- Is added to the top of its category in the stickied daily thread.
- Receives a gem/diamond emoji next to its title in the stickied daily thread to make it stand out.
- Will receive new features that are added for promoted threads in the future.

Promoting a thread currently costs $2.50 (USD) per month. BTC, LTC, and ETH are accepted.

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

# Confirming an Order

To confirm an order from a sales thread (for a buyer) or for promoting a sales thread (for a seller), the `confirm order` command is used.

**Syntax**
```
confirm order :orderId: with transaction :transactionHash:
```

**Example**
```
confirm order 54321 with transaction 955319683a1eae2a519495217096fc1bb9fe171287284f79f37d49624835bbc0
```

Make sure you know how to find your transaction hash in the wallet you use before making a payment. Orders must be confirmed with this command within 15 minutes of the order being created! Once confirmed through this command, it may take up to an hour for the transaction to receive enough confirmations on the blockchain for the payment to be accepted.

# Giving Feedback

Give feedback for a completed order.

**Syntax**
```
give positive|negative feedback for :orderId: <optional message here>
```

**Example**
```
give positive feedback for 54321 Thanks, great seller/buyer!
```

# Getting a User's Stats Thread

Retrieve a link to a user's r/xyMarketStats thread, if one exists.

**Syntax**
```
get stats for u/:user:
```

**Example**
```
get stats for u/xyMarketBot
```

# Confirming a Trade

A trade is any transfer of one set of items/currency for another set of items/currency with another user that took place in a way that was *not* tracked by xyMarket, and is not considered on 'order'.

It is recommended that the seller or creator of a sales thread is the one to initiate a trade confirmation but in the end it doesn't really matter.

Trades can be confirmed privately via private message or publicly via comments. If you wish to confirm a trade publicly, it is recommended to do so on the sales thread itself, or on one of the user's stats threads. You can however, confirm a trade anywhere you like.

## Step 1

*Prefix the following commands with `u/xyMarketBot ` if you wish to confirm the trade publicly.*

**Syntax**
```
confirm trade of :item: to u/:user: for :item:
```

**Example**
```
confirm trade of $25 Amazon GC to u/xyMarketBot for $22 PayPal
```

## Step 2

Once step 1 has been completed, `:user:` will receive a 'confirm trade' request that they can use to finish the confirmation process.

*Prefix the following commands with `u/xyMarketBot ` if you wish to confirm the trade publicly.*

**Syntax**
```
confirm trade :tradeId: with u/:user:
```

**Example**
```
confirm trade 54321 with u/xyMarketDevBot
```