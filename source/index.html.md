---
title: Tripexpert API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://www.tripexpert.com'>Tripexpert</a>
  - <a href='https://www.tripexpert.com/api'>API Overview</a>
  - <a href='mailto:developer@tripexpert.com'>Email Support</a>

includes:

search: true
---

# Introduction

The Expert Review API provides access to professional review data for hotels, restaurants and tourist attractions from leading travel guides, magazines and newspapers.

**For more information about the API, including a summary of coverage and data available, and to request an API key, please visit our [API Overview](https://www.tripexpert.com/api).**


# Authentication

```shell
# Pass the API key as a URL parameter:
curl "https://api.tripexpert.com/v1/[endpoint]?api_key=xxx"
```

An API key is required to access to the API. Include your API key as the parameter `api_key` on all requests.


# Versioning

Minor revisions are made on an ongoing basis. Major backwards-incompatible changes are released as numbered versions. The current version of the API is accessible at the following endpoint:

`https://api.tripexpert.com/v1`

# Common Features

## Pagination

For the [Destination Index](#destination-index) and [Venue Index](#venue-index) endpoints, the following parameters can be used for pagination:

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Number of records to return
offset | 0 | Record on which to start

The total number of available records for a query is indicated in the response as `total_records`.


## Geo Queries

For the [Destination Index](#destination-index) and [Venue Index](#venue-index) endpoints, results can be ordered by distance from a particular coordinate by passing `latitude` and `longitude` and setting `order_by` to `distance`.

The distance in miles to the relevant result is returned as `distance`.    



# Errors

HTTP error codes are used to indicate the status of a request. Codes in the 2xx range indicate success, codes in the 4xx range indicate an error resulting from the parameters provided, and codes in the 5xx range indicate an error with our service.

The request status is also returned in the `meta` node in the response.


# Venue Mapping

We use our own internal identifiers to reference venues. If you need to establish correspondence between our venue identifiers and those of a third party service (such as Booking.com, TripAdvisor, or EAN), you have two options:
* Use our mapping endpoint, available to select users only, which accepts as input venue name and location information and returns likely matching venues.   
* We may be able to send you a flat text file with this data.
Please contact us for more information.


# Destinations

## Destination Index


```shell
curl "https://api.tripexpert.com/v1/destinations?order_by=distance&latitude=40.716236&longitude=-73.965237"

```

> Sample response snippet:

```json
{
	"id": "6",
	"name": "New York City",
	"country_name": "United States",
	"priority": 1,
	"permalink": "new-york-city",
	"index_photo": "https://static.tripexpert.com/images/destinations/index_photos/explore/6.jpg?1402263520",
	"distance": 3.4520683
}
```

This endpoint retrieves a list of destinations.

### HTTP Request

`GET https://api.tripexpert.com/v1/destinations`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
order_by | name | Sort order; options are `name`, `distance` and `priority` *
country_id | | [Country ID](#countries)
latitude |  | Required when `order_by` is `distance`
longitude |  | Required when `order_by` is `distance`

\* `priority` is an internal measure of the significance of a destination based on metrics such as demographics, number of venues and number of reviews.

# Venues

## Venue Index

```shell
curl "https://api.tripexpert.com/v1/venues?destination_id=6&amenity_ids[]=31&amenity_ids[]=32"

```

> Sample response snippet:

```json
{
  "id": 637972,
"venue_type_id": 1,
"destination_id": 6,
"name": "Greenwich Hotel",
"path": "new-york-city/hotels/greenwich-hotel",
"tripexpert_score": 95,
"rank_in_destination": 1,
"rank_as_percentage": 1,
"experts_choice": true,
"low_rate": 650,
"latitude": "40.719902",
"longitude": "-74.01018",
"address": "377 Greenwich St., New York City, NY 10013",
"telephone": "+1 646-679-6960",
"website": "http://www.thegreenwichhotel.com",
"live": true,
"star_rating": 5,
"amenities": [
{
"name": "Pet Friendly"
},
{
"name": "Bar/Lounge"
},
{
"name": "Free Internet"
},
{
"name": "Room Service"
},
{
"name": "Laundry Service"
},
{
"name": "Concierge"
},
{
"name": "Restaurant"
},
{
"name": "Pool"
},
{
"name": "Fitness Center"
},
{
"name": "Family Friendly"
},
{
"name": "Business Center"
}
]
}
```

This endpoint retrieves a list of venues in a specified destination.

### HTTP Request

`GET https://api.tripexpert.com/v1/venues`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
destination_id | | Destination ID
order_by | rank | Sort order. Options are `rank`, `tripexpert_score` and `distance`
latitude |  | Required when `order_by` is `distance`
longitude |  | Required when `order_by` is `distance`
venue_type_id |  | Options are `1` (hotels), `2` (restaurants), `3` (attractions)
amenity_ids |  | [Amenity IDs](#amenities) - supply as an array (see example at right)
category_ids |  | [Category IDs](#categories) - supply as an array
neighborhood_ids |  | [Neighborhood IDs](#neighborhoods) - supply as an array
price_category_ids |  | Options are `1` (budget hotels), `2` (midrange hotels), `3` (luxury hotels), `4` (budget restaurants), `5` (midrange restaurants), `6` (high-end restaurants) - supply as an array


## Venue Show

```shell
curl "https://api.tripexpert.com/v1/venues/636532?latitude=40.716236&longitude=-73.965237"

```

> Sample reviews response snippet (this endpoint will also retrieve all of venue metadata contained in the Venue Index response):

```json
"reviews": [
{
"id": 15669,
"publication_id": 1,
"publication_name": "Frommer's",
"extract": "Features individually designed rooms complete with hand-picked art and furniture combined with the newest in high-tech amenities.",
"display_rating": "[star][star][star]",
"source_url": "http://www.frommers.com/destinations/new-york-city/hotels/crosby-street-hotel",
"publication_rating_name": "3",
"normalized_score": 1
},
{
"id": 21665,
"publication_id": 6,
"publication_name": "Fodor's",
"extract": "Each of the rooms is individually designed, with fun patterns and colors, big warehouse style windows, and a cozy, residential feel. The location is smack in the heart of the action in SoHo.",
"display_rating": "[star][star][star][star][half-star]",
"source_url": "https://www.fodors.com/world/north-america/usa/new-york/new-york-city/hotels/reviews/crosby-street-hotel-474658",
"publication_rating_name": "4.5"
},
{
"id": 28180,
"publication_id": 2,
"publication_name": "Lonely Planet",
"extract": "It’s not just the clotted cream and raisiny scones that will grab you, but the fun and upbeat lobby... the whimsical striped chairs in the bar and the unique rooms.",
"display_rating": "[checkmark] Top Choice",
"source_url": "https://www.lonelyplanet.com/usa/new-york-city/soho-and-chinatown/hotels/crosby-street-hotel/a/poi-sle/1109962/1320600",
"publication_rating_name": "Top Choice",
"normalized_score": 1
},
{
"id": 41073,
"publication_id": 12,
"publication_name": "Travel + Leisure",
"extract": "As you might expect from a Soho property, Crosby Street Hotel oozes coolness.",
"display_rating": null,
"source_url": "https://www.travelandleisure.com/hotels-resorts/best-boutique-hotels-in-nyc"
},
{
"id": 342367,
"publication_id": 53,
"publication_name": "Forbes Travel Guide",
"extract": "The stateside outpost of U.K.-based Tim and Kit Kemp’s Firmdale hotel group boasts the same eclectic English country-meets-modernist design as its London sisters.",
"display_rating": "[checkmark] 4 Stars",
"advice": "One of the perks of staying at Crosby Street Hotel is having access to the private, guest-only areas. ",
"source_url": "https://www.forbestravelguide.com/hotels/new-york-city-new-york/crosby-street-hotel",
"publication_rating_name": "4 Stars"
},
{
"id": 416835,
"publication_id": 8,
"publication_name": "Condé Nast Traveler",
"extract": "Aside from being a visual treat, the location is nearly perfect—a quiet block in the heart of SoHo, next to public transportation, but also pleasingly walkable.",
"display_rating": null,
"source_url": "https://www.cntraveler.com/hotels/united-states/new-york/crosby-street-hotel-new-york"
},
{
"id": 471811,
"publication_id": 55,
"publication_name": "Afar Magazine",
"extract": "Rooms feature floor-to-ceiling warehouse-style windows, with gorgeous views over SoHo and lower Manhattan.",
"display_rating": null,
"source_url": "https://www.afar.com/places/crosby-street-hotel"
},
{
"id": 654902,
"publication_id": 61,
"publication_name": "The Telegraph",
"extract": "Inside it’s all understated elegance.",
"display_rating": "9.0",
"source_url": "https://www.telegraph.co.uk/travel/destinations/north-america/united-states/new-york/hotels/crosby-street-hotel/",
"publication_rating_name": "9"
},
{
"id": 655036,
"publication_id": 63,
"publication_name": "Departures",
"extract": "A mainstay for Hollywood elite during the Tribeca Film Festival—and appreciated by anyone with an interest in contemporary art.",
"display_rating": null
},
{
"id": 658299,
"publication_id": 64,
"publication_name": "goop",
"extract": "This exuberant Firmdale Hotels offering is sort of the perfect mix of over-the-top design flourishes and straight-up excellent hospitality, which makes it an instant hit for kids.",
"display_rating": null,
"source_url": "https://goop.com/place/new-york/new-york-city/soho-hotels/crosby-street-hotel/"
},
{
"id": 706979,
"publication_id": 66,
"publication_name": "Mr & Mrs Smith",
"extract": "Retreating to a luxurious, cool oasis after spending a day rabble-rousing in one of the most energetic cities in the world is pretty unbeatable.",
"display_rating": null,
"advice": "The Junior Suites on the 10th and 11th floors have a winning combination of height and huge warehouse windows",
"source_url": "https://www.mrandmrssmith.com/luxury-hotels/crosby-street-hotel"
},
{
"id": 936795,
"publication_id": 75,
"publication_name": "The Times",
"extract": "A great base to explore the cool funky end of Manhattan, the Crosby attracts an artsy crowd, hanging out on the breezy restaurant terrace and the hotel cinema’s Sunday night film club.",
"display_rating": null,
"source_url": "https://www.thetimes.co.uk/travel/destinations/north-america/us/new-york-city/best-luxury-hotels-in-new-york",
"publication_article": {
"title": "Best luxury hotels in New York",
"date_published": "2022-05-24"
}
}
]
```

This endpoint retrieves details for a specified venue. This includes all of the reviews available for the venue (see sample response at right). This is the primary API method for returning expert review data.

<aside class="notice">
The number of reviews that are returned and the nature of information returned for each review depends on your level of API access.
</aside>

If `latitude` and `longitude` are supplied, the response will include the distance to the venue (in miles).

### HTTP Request

`GET https://api.tripexpert.com/v1/venues/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | Venue ID  
latitude |  
longitude |  



# Hotel Pricing

```shell
curl "https://api.tripexpert.com/v1/venues?destination_id=6&limit=5&check_in=01/01/2016&check_out=01/02/2016"

```

> Sample response snippet:

```json
{
	"availability": "true",
	"rates": [
		{
			"provider": "Booking.com",
			"rate": "$4,500",
			"url": "https://www.booking.com/hotel/us/crosby-street.html?aid=381942&checkin_monthday=1&checkin_year_month=2016-01&checkout_monthday=2&checkout_year_month=2016-01"
		}
			]
}
```
The  [Venue Index](#venue-index) and [Venue Show](#venue-show) endpoints can also be used to return information about hotel room availability and pricing from Tripexpert's OTA partners. This is done by passing (at a minimum) `check_in` and `check_out` dates.

<aside class="notice">
These methods are not available to all API users.
</aside>

### URL Parameters

In addition to the general parameters for the [Venue Index](#venue-index) or [Venue Show](#venue-show) calls:

Parameter | Default | Description
--------- | ------- | -----------
check_in | | Check-in date, mm/dd/yyyy
check_out | | Check-out date, mm/dd/yyyy
rooms | 1 | Number of rooms
guests | 2 | Number of guests
currency | USD | Currency code

<aside class="warning">
When using requesting pricing and availability data for multiple hotels, performance declines rapidly; avoid requesting more than 10 results at once.
</aside>





# Publications

```shell
curl "https://api.tripexpert.com/v1/publications"
```

> Sample response snippet:

```json
{
	"id": 2,
	"publication_name": "Lonely Planet"
}
```
This endpoint returns a list of all of our source publications.

### HTTP Request

`GET https://api.tripexpert.com/v1/publications`






# Amenities

```shell
curl "https://api.tripexpert.com/v1/amenities"

```

> Sample response snippet:

```json
{
"id": 4,
"name": "Hot Tub"
}
```
This endpoint returns a list of hotel amenities. The amenity IDs can be used as filters for [Venue Index](#venue-index) queries.

### HTTP Request

`GET https://api.tripexpert.com/v1/amenities`





# Categories

```shell
curl "https://api.tripexpert.com/v1/categories?venue_type_id=2"
```

> Sample response snippet:

```json
{
"id": 8,
"name": "French"
}
```
This endpoint returns a list of categories. It takes a single parameter, `venue_type_id`.

Categories are cuisines for restaurants (`venue_type_id` 2) and classifications such as "museum", "outdoors", "historical" for amenities (`venue_type_id` 3). Hotels (`venue_type_id` 1) do not have categories.

The category IDs can be used as filters for [Venue Index](#venue-index) queries.

### HTTP Request

`GET https://api.tripexpert.com/v1/categories`





# Neighborhoods

```shell
curl "https://api.tripexpert.com/v1/neighborhoods?destination_id=6"
```

> Sample response snippet:

```json
{
"id": 24,
"name": "Lower East Side"
}
```
This endpoint returns a list of neighborhoods. It takes a single parameter, `destination_id`. Neighborhoods are not available for all destinations. The neighborhood IDs can be used as filters for [Venue Index](#venue-index) queries.

### HTTP Request

`GET https://api.tripexpert.com/v1/neighborhoods`






# Countries

```shell
curl "https://api.tripexpert.com/v1/countries"

```

> Sample response snippet:

```json
{
"id": 64,
"name": "Bhutan"
}
```
This endpoint returns a list of countries. The country IDs can be used as filters for [Destination Index](#destination-index) queries.

### HTTP Request

`GET https://api.tripexpert.com/v1/countries`
