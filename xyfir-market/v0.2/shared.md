This sections contains only documentation that is relevant to both buyers and sellers. Please check the [buyer](https://www.xyfir.com/documentation/xyfir-market/buyers) and/or [seller](https://www.xyfir.com/documentation/xyfir-market/sellers) docs for more information!

# Confirm Order

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

# Give Feedback

Give feedback for a completed order.

**Syntax**
```
give positive|negative feedback for :orderId: <optional message here>
```

**Example**
```
give positive feedback for 54321 Thanks, great seller/buyer!
```

# Get User Stats

Retrieve a link to a user's r/xyMarketStats thread, if one exists.

**Syntax**
```
get stats for u/:User:
```

**Example**
```
get stats for u/xyMarketBot
```