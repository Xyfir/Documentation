# API Reference
`GET https://ads.xyfir.com/api/ads`

---

## Required Query Data
- `pubid`: *number* - Your campaign's id number
- `type` OR `types` - See [Ad Types](#AdTypes)
    - `type`: *number* - Numerical value representing advert type you want (text/video/etc)
    - `types`: *string* - A comma delimited list of the ad types you want. If you want a specific amount of ads with each ad type we recommend making multiple requests for each type with ?type and ?count.

## Optional Query Data
- `count`: *number* - (default 1), How many ads you want returned
- `xadid`: *string* - User's Xyfir Ads id received when user logs into your service via Xyfir Accounts
- `speed`: *number* - (default 0), If set to 1: certain filters and checks will be ignored. The result is a faster response at the cost of less personalized ads
- `ip`: *string* - (default uses request IP), If your server acts as a middle-man between your client and Xyfir Ads: this should be set to the client's IP address
- `age`: *number* - User's age range (See [Age Ranges](#AgeRanges))
- `gender`: *number* - User's gender (See [Genders](#Genders))
- `categories`: *string* - A comma delimited list of categories (must be in our list of approved categories) that describe the page/content/etc the user is currently viewing
- `keywords`: *string* - A comma delimited list of keywords that describe the page/content/etc the user is currently viewing
- `test`: *string* - See [Test Mode](#TestMode)

## Return Object
```
{ ads: [
    {
        type: number, link: string, title: string,
        description: string, media?: string
    }, 
] }
```

### Media Property
This property is optional and only available for image and video advertisements. It is a comma delimited key value list `(key:value,key:value,...)` where the key is a numerical identifier matching a preset image/video dimensions/resolution and the value is a link to the actual content. See [Image Source Types](#ImageSourceTypes) and [Video Source Types](#VideoSourceTypes)

#### Notes
- Be sure to check the length of response.ads as it's possible we were unable to find enough suitable matches to meet your `count` value
- Also be sure to check the type of the ads returned if you provided a list of ad types with `types` instead of a single type with `type`

## Test Mode #TestMode
- When developing and testing with our API you should be sure to enable test mode. Test mode is enabled by providing your test key (found when viewing your campaign) via `GET ads.xyfir.com/api/ads?pubid=XX&test=XX`. We will not charge advertisers for views or clicks and you won't receive earnings for ads served in test mode. Reports for both publisher and advertiser will not be updated.
- We highly recommend keeping your test key private, and generating a new key if your old one is made public. It is important to keep private since it prevents you from earning anything when provided to our API. You can generate a new key under the 'Edit' menu of each campaign.
- Each campaign has its own unique test key. The test key and pubid must match for test mode to be enabled. There are no warnings or errors when they don't match.

---

## Preventing Blocked Ads

### Elements
The actual ad elements in your page's DOM should not contain class names or ids for easy selecting. In addition to this you should frequently rearrange your ad's location in the DOM.

### Retrieving Ads
The code that retrieves and builds the ads into your page should not be separate from the rest of the code that runs your site's front-end. Having this code separate makes it exceptionally easy to block. You should bundle in the code that runs the ads with the code that runs your site. The more tightly integrated the ad retrieval and build system is with the rest of your site's functions, the harder it is to block.

### Middle-Man System
A middle-man system is useful for preventing the blocking of external scripts and blocking known IP addresses for Xyfir Ads. This system ensures that all ads are retrieved and provided from the same source as the rest of the website's content (your website, not ours).
There is no premade middle-man system as it's implementation is unique to all systems and needs, however we can give you an outline to start from.

- Instead of calling our API from the client, call your own API that acts as a middle-man. This API will take the request from the client, add onto it and modify it, and then will send the request to Xyfir Ads' API. Once your API receives a response from our API, this system will then analyze the response and determine any appropriate actions to take before ultimately returning the ad to your user's client.

#### Masking Ad Links:
- Having links on your website to Xyfir Ads is a sure way to get those ads blocked. The work around is masking those links with links to your own site that then redirect to Xyfir Ads.
- When your API receives the response from Xyfir Ads, it should loop through all ads returned and generate links on your own site that redirect to the advertisement's links. Once a redirect link is generated, it should replace the real link in the ads object before being returned to the client. Its important to note that a pattern could be detected in the format for the links you use to serve ads. Due to this, it is important you change up the format of the ad redirect links as often as possible.

#### Masking Videos and Images:
- Like ad links, pulling images and videos from Xyfir Ads is a good way to get your ads blocked. The solution again, is to make the images and videos come from the same source as the rest of the content on the page.
- When your API retrieves a video or image advertisement it should loop through all of the media sources. For each image/video it should download the file to your network and modify the link in the ads object to come from your source, instead of ours. This way the user's client sees video and image ads coming from the exact same source as the rest of the content on your page.

---

## Ad Types #AdTypes

### Shared Properties
- `description`: *string* - 150 characters
- `title`: *string* - 25 characters
- `type`: *number*
- `link`: *string*

### Text
- Numerical Identifier: **1**

### Short Text
- Numerical Identifier: **2**
- Properties:
    - While short text ads have the title and description properties shared by the other ads there is an important different in the lengths
    - `title` - 15 characters
    - `description` - 25 characters

### Image
- Numerical Identifier: **3**
- Properties
    - `media`: *string* - `0:sourceLink,1:...,3:sourceLink`
    - See [Image Source Types](#ImageSourceTypes)

### Video
- Numerical Identifier: **4**
- Properties
    - `media`: *string* - `0:sourceLink,1:...,3:sourceLink`
    - See [Video Source Types](#VideoSourceTypes)

## Image Source Types #ImageSourceTypes
- Each key:value pair in the media property of an ads object represents an image of certain preset dimensions. Below you'll find the image dimensions that match each key.
- Key $X: $Yx$Z

## Video Source Types #VideoSourceTypes
- Each key:value pair in the media property of an ads object represents a video of a certain preset resolution. Below you'll find the video resolution that match each key.
- Key $X: $Yx$Z

## Age Ranges #AgeRanges
- Each age range has a specific numerical identifier used throughout our system.
- 18-24: **1**
- 25-34: **2**
- 35-44: **3**
- 45-54: **4**
- 55-64: **5**
- 65+: **6**

## Genders #Genders
- Each gender has a specific numerical identifier used throughout our system.
- Male: **1**
- Female: **2**
- Other: **3**