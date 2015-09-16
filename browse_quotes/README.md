# Skyscanner Browse Quotes API: Quick Start

Use this API to obtain indicative prices for flights over a range of dates and/or routes. The examples below show what kind of question the API can answer.

This is a quick-start guide. More comprehensive documentation is available in the [full documentation].

#### Prerequisites

* A Skyscanner API key.
* Understanding of the HTTP protocol.

#### Definition of Browse Prices

The prices shown by the browse API are the cheapest price per market and route that Skyscanner has seen in the last 15 days. Use it for indicative pricing solutions e.g. "give me an idea of how much it costs to fly to... on...".

The browse API is **not:**

* A cache of the most recent price of an itinerary.
* Guaranteed to be accurate.
* Guaranteed to be complete.

#### Workflow

Make a GET request with your desired travel parameters. You can use HTTPS if you prefer. You may send the `Accept` header in your request to get responses in `application/json` or `application/xml`.

#### URL Format and Example Request

``` http
http[s]://api.skyscanner.net/apiservices/browsequotes/v1.0/{country}/{currency}/{locale}/{origin}/{destination}/{outbound_date}[/{inbound_date}]?apikey=YOUR_API_KEY_HERE
```

JSONP-friendly URL:
``` http
http[s]://api.skyscanner.net/apiservices/xd/browsequotes/v1.0/{country}/{currency}/{locale}/{origin}/{destination}/{outbound_date}[/{inbound_date}]?apikey=YOUR_API_KEY_HERE&callback=myJsMethod
```

* `country` valid values (typically the country in which the ticket purchaser is resident): See [countries reference]
* `currency` valid values: See [currencies reference]
* `locale` valid values: See [locales reference]
* `origin` and `destination` valid values:
  * `anywhere` for quotes by country
  * Two-letter country code e.g. `US` for quotes by each city in the US
  * Skyscanner city code e.g. `NYCA-sky` for quotes by each airport in New York City (look up codes using autosuggest; see [full documentation])
  * IATA airport code e.g. `JFK` for quotes specific to JFK airport
  * `188.39.95.93-ip` to geolocate by IP
  * `37.678,-122.452-latlong` to geolocate by latitude and longitude
* `outbound_date` and (optionally) `inbound_date` valid values:
  * `anytime` for quotes by month until a year from now
  * `yyyy-mm` for quotes by day in the month you specify e.g. `2015-12` for December 2015
  * `yyyy-mm-dd` for quotes specific to that date

A request might look like:

``` http
GET http://api.skyscanner.net/apiservices/browsequotes/v1.0/UK/GBP/en-GB/EDI-iata/SFO-iata/2015-11?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: api.skyscanner.net
Accept: application/json
```
  
### Further Examples
  
##### Indicative prices from every city in the US to Tokyo in November 2015.

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/US/USD/en-US/US/TYOA-sky/2015-11?apikey=YOUR_API_KEY_HERE`

##### What's the cheapest way to fly from SFO to Greece? I don't care when.

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/US/USD/en-US/US/GR/anytime/anytime?apikey=YOUR_API_KEY_HERE`

##### Take me from BOS to SFO on 2015-09-30 and return on 2015-10-03.

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/US/USD/en-US/BOS-iata/SFO-iata/2015-09-30/2015-10-03?apikey=YOUR_API_KEY_HERE`

##### I'm free the weekend of 2015-10-09 to 2015-10-11, just take me anywhere from a London airport.

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/UK/GBP/en-GB/LOND-sky/anywhere/2015-10-09/2015-10-11?apikey=YOUR_API_KEY_HERE`

##### What are the cheapest ways to fly to every country in the world from the US?

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/US/USD/en-US/US/anywhere/anytime/anytime?apikey=YOUR_API_KEY_HERE`

##### I want to go from any New York City airport to Vancouver International Airport, outbound and inbound some time in April 2016.

`http://api.skyscanner.net/apiservices/browsequotes/v1.0/CA/CAD/en-GB/NYCA-sky/YVR-iata/2016-04/2016-04?apikey=YOUR_API_KEY_HERE`

#### Known Gotchas

* Got a `403` error? Did you remember to append your `apiKey` to the query string?

  [full documentation]: <http://business.skyscanner.net/portal/en-GB/Documentation/ApiOverview>
  [currencies reference]: <http://business.skyscanner.net/portal/en-GB/Documentation/Currencies>
  [locales reference]: <http://business.skyscanner.net/portal/en-GB/Documentation/Locales>
  [countries reference]: <http://business.skyscanner.net/portal/en-GB/Documentation/Markets>