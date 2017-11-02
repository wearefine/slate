# API

Rails 5 API App to access SMWE's data, includes a RESTful API to expose certain data from the DB to the public.

| Rails Version | Ruby Version | Run Tests |
|------------|-----------|---------|
| 5.1.4 | 2.4.2  | Guard |

## API

This application includes a RESTful API to expose certain data from the DB to the public.

### Access Token

To access any aspect of the API you'll need an access token. Contact FINE if you don't have one.

In every request to the API include you access token as an Authorization header. Here are a couple of examples with an access_token of `12345`:

**cURL**

```shell
curl "http://api.smwe.com/v1/CSM/retailers?location=97217"
     -H 'Authorization: Token token="12345"'
```


**Ruby REST Client**

```ruby
require 'rest_client'
RestClient.get 'http://api.smwe.com/v1/CSM/retailers',
  location: '97217',
  authorization: 'Token token="12345"'
```

<aside class="notice">
http vs https
<p>If the application connecting to the API will be hosted on the SMWE servers use `http` as connections will be made via internal IPs. Otherwise use `https`.</p>
</aside>

## Retailers

### GET /v1/:brand/retailers

Returns a list of retailers for a brand and location, ordered by distance.
":brand" is the three digit brand code.

### Parameters

| param      | required? | default | description |
|------------|-----------|---------|-------------|
| location   | yes       |         | A zip, address, city or landmark     |
| product    |           |         | The 7-9 digit alphanumeric product key, filters retailers to only those with product |
| radius     |           | 50      | The area to search for retailers in miles|
| pagination |           | true    | Paginates results |
| page       |           | 1       | Page number to return |
| per_page   |           | 5       | Number of results returned per page |
| on_premise |           |         | "true" or "false", show only retailers that do or don't serve on premise, leave blank to show all |

#### Response

```shell
curl "http://api.smwe.com/v1/CSM/retailers?location=97217&page=2"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
{
  "total_results": 1491,
  "total_pages": 299,
  "page": "2",
  "retailers": [
    {
      "code": 126353,
      "name": "Walgreens",
      "address": "2829 N Lombard St",
      "city": "Portland",
      "state": "OR",
      "zip": "97217",
      "lon": -122.697,
      "lat": 45.5772,
      "on_premise": false,
      "distance": 0.56,
      "products": [
        {
          "product_key": "CSMT1CB1",
          "brand_code": "CSM",
          "product_code": "CB1",
          "description": "Columbia Valley Cabernet Sauvignon"
        },
        {
          "product_key": "CSMT1RI1",
          "brand_code": "CSM",
          "product_code": "RI1",
          "description": "Columbia Valley Riesling"
        }
      ]
    },
    // ... 4 more retailers
  ]
}
```

## Distributors

### GET /v1/:brand/distributors

Returns a list of distributors for a brand and country, ordered by state and city.
":brand" is the three digit brand code.

### Parameters

| param      | required? | default | description |
|------------|-----------|---------|-------------|
| country    | yes       |         | A two digit country code |
| pagination |           | true    | Paginates results |
| page       |           | 1       | Page number to return |
| per_page   |           | 5       | Number of results returned per page |

### Response

```shell
curl "http://admin2.smwe.com/v1/CSM/distributors?country=CA"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
{
  "total_results": 15,
  "total_pages": 3,
  "page": 1,
  "distributors": [
    {
      "jde_distributor_number": 36431,
      "brand_code": "CSM",
      "name": "ALBERTA LC AUTHENTIC(C/O CONTAINERWORLD)",
      "city": "RICHMOND",
      "state": "AB",
      "country": {
        "iso": "CA",
        "name": "CANADA",
        "printable_name": "Canada",
        "iso3": "CAN",
        "numcode": 124
      },
      "zip": "V6W 0A3",
      "phone": "604-540.2582",
      "url": "www.awsm.ca\r\n"
    },
    // ... 4 more distributors
  ]
}
```

## Products

### GET /v1/:brand/products

Returns a list of products with retailers for a brand.
":brand" is the three digit brand code.

### Response

```shell
curl "http://api.smwe.com/v1/CSM/products"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
[
  {
    "product_key": "CSMT1MR1",
    "brand_code": "CSM",
    "product_code": "MR1",
    "description": "Columbia Valley Merlot"
  },
  {
    "product_key": "CSMT1RI1",
    "brand_code": "CSM",
    "product_code": "RI1",
    "description": "Columbia Valley Riesling"
  },
  // ...
]
```

## Countries

### GET /v1/:brand/countries

Returns a list of countries with distributors for a brand.
":brand" is the three digit brand code.

### Response

```shell
curl "http://api.smwe.com/v1/CSM/countries"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
[
  {
    "iso": "AW",
    "iso3": "ABW",
    "name": "ARUBA",
    "printable_name": "Aruba",
    "numcode": 533
  },
  {
    "iso": "AU",
    "iso3": "AUS",
    "name": "AUSTRALIA",
    "printable_name": "Australia",
    "numcode": 36
  },
  {
    "iso": "BS",
    "iso3": "BHS",
    "name": "BAHAMAS",
    "printable_name": "Bahamas",
    "numcode": 44
  },
  // ...
]
```

## Events List

### GET /v1/:brand/events

Returns a list of current events, live on production, scoped by brand, ordered ascending chronologically.
":brand" is the three digit brand code.

### Parameters

| param          | required? | default | description |
|----------------|-----------|---------|-------------|
| on_development |           | false   | Display non-production data |
| show_past      |           | false   | Show past events |
| event_type     |           |         | Event Type to return |
| pagination     |           | false   | Paginates results |
| page           |           | 1       | Page number to return, if pagination true |
| per_page       |           | 5       | Number of results returned per page, if pagination true |


### Response

```shell
curl "http://api.smwe.com/v1/CSM/events"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
[
  {
    "id": 2155,
    "event_type":
      {
        "id": 6,
        "name": "Culinary Event"
      },
    "title": "Washington Harvest Chef Dinner Italian Style!",
    "slug": "washington-harvest-chef-dinner-italian-style",
    "date_start": "2017-09-30",
    "date_end": "2017-09-30",
    "formatted_date": "Sep 30, 2017",
    "schedule": "7:00pm",
    "introduction": "Come celebrate Chateau Ste. Michelle’s 50th Anniversary by joining Chef Scott Harberts as he showcases the beautiful fall harvest of Washington State.  ",
    "is_featured": true,
    "image": "/files/brands_cms/FlexibleImage/10876/CSM_HarvestDinner_Sept2017_V2__1_.jpg",
    "image_alt": ""
  },
  {
    "id": 2236,
    "event_type":
      {
        "id": 47,
        "name": "Grand Vin Events"
      },
    "title": "Washington Harvest Chef Dinner Italian Style!",
    "slug": "washington-harvest-chef-dinner-italian-style-gv",
    "date_start": "2017-09-30",
    "date_end": "2017-09-30",
    "formatted_date": "Sep 30, 2017",
    "schedule": "7:00pm",
    "introduction": "",
    "is_featured": false,
    "image": "/files/brands_cms/FlexibleImage/11181/CSM_HarvestDinner_Sept2017_V2__1_.jpg",
    "image_alt": ""
  },
  {
    "id": 2209,
    "event_type":
      {
        "id": 4,
        "name": "Vintage Reserve Club"
      },
    "title": "Heritage Reserve Release Party",
    "slug": "heritage-reserve-release-party",
    "date_start": "2017-10-08",
    "date_end": "2017-10-08",
    "formatted_date": "Oct 08, 2017",
    "schedule": "6:00 - 8:00pm",
    "introduction": "Complimentary for Heritage Members (up to 4 tickets)",
    "is_featured": false,
    "image": "/files/brands_cms/FlexibleImage/11087/HERITAGEparty.jpg",
    "image_alt": ""
  }
```

## Events Detail

### GET /v1/:brand/events/2155

Returns an active event.
":brand" is the three digit brand code.

### Response

```shell
curl "http://api.smwe.com/v1/CSM/events/2155"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
{
  "id": 1980,
  "seo_pagetitle": "Murder Mystery Dinner Event | Chateau Ste. Michelle",
  "seo_description": "\u003cstrong\u003eTickets will go on sale September 28th!\u003c/strong\u003e\r\n\u003cbr\u003e\u003cbr\u003e\r\n\u003cCome join us for a night back in the Roaring 20’s spiked with Murder, Mystery and Mayhem at Chateau Ste. Michelle. \r\n",
  "title": "Murder Mystery Dinner Event",
  "event_type":
    {
      "name": "Culinary Event"
    },
  "date_start": "2017-10-28",
  "date_end": "2017-10-28",
  "formatted_date": "Oct 28, 2017",
  "schedule": "6:30pm",
  "location": "Chateau Ste. Michelle Banquet Room",
  "introduction": "\u003cstrong\u003eTickets will go on sale September 28th!\u003c/strong\u003e\r\n\u003cbr\u003e\u003cbr\u003e\r\n\u003cCome join us for a night back in the Roaring 20’s spiked with Murder, Mystery and Mayhem at Chateau Ste. Michelle. \r\n",
  "video_url": "",
  "video_caption": "",
  "body": "You will be enjoying a wonderful meal paired with delicious award winning wines while solving an unfathomable murder! Whether you are playing a part, watching and laughing, enjoying a savory three course dinner, or working to solve the crime, it will be a fun filled evening! So bring a date or a group of friends and we hope to see you there.\r\n\u003cbr\u003e\u003cbr\u003e\r\nThere will be several prizes to be given out. Be ready to participate, come in character and dressed to impress in your finest 1920’s attire!\r\n\u003cbr\u003e\u003cbr\u003e\r\n\u003cu\u003eWhat to Expect:\u003c/u\u003e\r\n\u003cbr\u003e6:30pm - Guest Arrival, Check in, Enjoy Appetizers, Activities\r\n\u003cbr\u003e7:00pm - Seated for 3 Course Dinner \u0026 Let the Mystery Begin!\r\n\u003cbr\u003e9:00pm - Murder Solved and Event Concludes\r\n\u003cbr\u003e9:30pm - Awards\r\n\u003cbr\u003e\u003cbr\u003e\r\n\u003cem\u003ePerformance by: The Murder Mystery Company\u003c/em\u003e\r\n \u003cbr\u003e\u003cbr\u003e\r\n21 and over only, please.",
  "price_information": "$95 retail | $85 VRC members",
  "purchase_message": "",
  "url_purchase": "",
  "url_target": "",
  "title_url": "",
  "file": "",
  "image": "/files/brands_cms/FlexibleImage/10066/CSM_20s_Murder_Mystery_Dinner.jpg",
  "image_alt": "",
  "image_caption": ""
}
```

## Event Types

### GET /v1/:brand/event_types

Returns rank ordered active event types with associated wines, for use on events list endpoint filtering.
":brand" is the three digit brand code.

### Response

```shell
curl "http://api.smwe.com/v1/CSM/event_types"
     -H 'Authorization: Token token="12345"'
```
> The above command returns JSON structured like this:

```json
[
  {
    "id": 13,
    "name": "Winery Event"
  },
  {
    "id": 4,
    "name": "Vintage Reserve Club"
  },
  {
    "id": 6,
    "name": "Culinary Event"
  },
  {
    "id": 34,
    "name": "Winery Closures \u0026 Special Hours"
  }
]
```
