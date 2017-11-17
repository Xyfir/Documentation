# Sales Threads

In this section we'll explain what all the fields and values on a sales thread mean.

- **Price** - The price of the item being sold in USD. Why USD? Because it is stable, widely used, and easily converted to fiat or crypto currencies. USD is always used as the fixed price point, but not always accepted directly by the seller.
- **Sold By** - The user who is selling the item, followed by a link to their r/xyMarketStats thread.
- **Description** - The seller's description of what they're selling.
- **Type** - The type of item being sold. Has four possible values: `Digital Item`, `Digital Service`, `Physical Item`, `Physical Service`.
- **Category** - The category for the item being sold.
- **Currency** - A list of currencies the seller is accepting.
- **Stock** - An optional field that is present if the seller has **Autobuy** enabled. This field will show the number of autobuy items currently available in xyMarketBot's inventory. You can still purchase from an autobuy thread if the stock is less than the quantity you purchase, however the seller will have to manually send you the items you're owed.
- **Tracking** - If `Yes`, you can optionally decide to make a 'tracked' purchase using BTC, LTC, or ETH. This means that xyMarketBot generates an order, and then tracks the payment for the order to the seller's address. You will be able to give and receive feedback once the order is complete. Making trackable purchases allows you to build your reputation and also acts as a deterrent for bad sellers because they know they'll receive negative feedback if they do anything wrong. Tracked purchases do not cost extra.
- **Verified** - If this value is `Yes`, it means that a moderator has verified that the seller has somehow proven to a xyMarket moderator that they do indeed have what they are selling. This should add an extra level of assurance for potential buyers, however you should still deal with caution. Moderators, like anyone else, can be fooled.
- **Autobuy** - If `Yes` and you choose to make a purchase using BTC, LTC, or ETH, the items you buy will automatically be sent to you after purchase without having to wait for the seller to manually send you whatever you purchased. This only works for `Digital Item` type sales threads. See **Stock**.
- **Escrow** - If `Yes`, you can optionally decide to use escrow for your BTC, LTC, or ETH purchase. See the Escrow section below.
- **Purchase Links** - You can use the purchase links to easily build the purchase command to send to xyMarketBot. Simply click the link you want and then update the quantity from the default `1` to however many items you wish to buy.

# Making a Purchase

You can purchase an item publicly by commenting the `purchase` or `buy` command on the thread itself, or you can do it privately by sending xyMarketBot a private message with the (slightly modified) `purchase` or `buy` command. There is no difference in the outcome other than one is public and one is private.

Purchases should only go through xyMarketBot if multiple conditions are met: the sales thread is structured and has `**Tracking** True`; you want to make a purchase with BTC, LTC, or ETH; and the sales thread accepts the currency you want to make a purchase with. If any of those are not the case, you must contact the seller directly to make a purchase.

## Command

**On Thread Syntax**
```
u/xyMarketBot purchase|buy :quantity: using BTC|LTC|ETH[ and escrow]
```

**On Thread Example:**
```
purchase 1 using ETH and escrow
```

**In Private Message Syntax**
```
purchase|buy :quantity: of :threadId: using BTC|LTC|ETH[ and escrow]
```

**In Private Message Example**
```
buy 2 of 6s1psl using BTC
```

## Payment

You will then receive an address to send your payment to and an amount in the chosen currency to send. If you send the correct amount (exactly at or over the specified amount) within 15 minutes the order will be considered paid, otherwise it will be expire.

## Autobuy

If the sales thread has Autobuy enabled, you will receive your item immediately after xyMarketBot receives notice of your order being paid.

## Escrow

**NOTE:** Escrow is currently disabled while we determine its feasiblity and consider other possible implementations.

If you are making a purchase using escrow, you will be charged an extra fee that is equal to 1% of the price of the item or service you are purchasing `((price * quantity) * 0.01)`. In the event that you request a refund and a xyMarket moderator chooses to grant your refund, the 1% fee will *not* be a part of the refunded amount.

The seller will *not* receive their funds until you send the `release escrow` command for the order. Release the funds once you've received your item(s) and are satisfied with the order. Additionally, you may be notified that the seller has requested you to release the escrow.

Both you and the seller can contact a moderator at any time in attempt to resolve a dispute. Once a moderator is involved, they will ask for proof to back up your claims, and then they will side with either you or the seller. Once a moderator releases the funds to the seller or returns it to you the order will be completed.

# Release Escrow

Release funds held in escrow for order to seller.

**Syntax**
```
release escrow for :orderId:
```

**Example**

```
release escrow for 54321
```