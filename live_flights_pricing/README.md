# Skyscanner Live Flights API: Quick Start

Use this API to obtain live flight prices and deeplinks to airlines or online travel agents (OTAs) where you may book tickets.

This is a quick-start guide. More comprehensive documentation is available in the [full documentation].

#### Prerequisites

* A Skyscanner API key.
* Understanding of the HTTP protocol.
* Understanding of the REST API model.

#### Workflow

First, create a session with your desired travel parameters. Then, poll this session several times for results. An example is given below.

#### Step 1: Create the Session

``` http
POST http://business.skyscanner.net/apiservices/pricing/v1.0/?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: business.skyscanner.net
Content-Type: application/x-www-form-urlencoded; charset=UTF-8

country=US&currency=USD&locale=en-US&locationSchema=iata&grouppricing=true&originplace=SFO-iata&destinationplace=BOS-iata&outbounddate=2015-09-23&inbounddate=2015-09-30&adults=1&children=0&infants=0&cabinclass=Economy
```

``` http
HTTP/1.1 201 Created
Date: Tue, 15 Sep 2015 13:45:33 GMT
Location: http://business.skyscanner.net/apiservices/pricing/uk1/v1.0/7ba59ba5863a454db96229ecb2bafe7c_ecilpojl_54BB3A9DB131D06736C557B8F95996A6
```

#### Step 2: Poll the Session several times

``` http
GET http://business.skyscanner.net/apiservices/pricing/v1.0/7ba59ba5863a454db96229ecb2bafe7c_ecilpojl_54BB3A9DB131D06736C557B8F95996A6?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: business.skyscanner.net
Accept: application/json
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 15 Sep 2015 13:45:36 GMT
```
_(Response body and some headers removed for brevity.)_

``` http
GET http://business.skyscanner.net/apiservices/pricing/v1.0/7ba59ba5863a454db96229ecb2bafe7c_ecilpojl_54BB3A9DB131D06736C557B8F95996A6?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: business.skyscanner.net
Accept: application/json
```

``` http
HTTP/1.1 304 Not Modified
Date: Tue, 15 Sep 2015 13:45:38 GMT
```
_(No body is returned with this response code.)_

``` http
GET http://business.skyscanner.net/apiservices/pricing/v1.0/7ba59ba5863a454db96229ecb2bafe7c_ecilpojl_54BB3A9DB131D06736C557B8F95996A6?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: business.skyscanner.net
Accept: application/json
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 15 Sep 2015 13:45:40 GMT
```
_Response body JSON document available here: [sample_live_flights_pricing_response.json]_

#### Notes
* Several session polls may be required and you should wait 1-2 seconds between each.
* `200` and `304` responses indicate success. These status codes are widely documented online.
* Continue polling while the body of the polling response contains `"Status": "UpdatesPending"`.
* Stop polling when the body of the polling response contains `"Status": "UpdatesComplete"`.
* Sessions expire after 30 minutes.
* Do not re-use sessions for different users.
* Do not cache deeplinks for future use by different users.
* Always append your `apiKey` to the query string.
* Your API key is subject to rate limiting. If you exceed the rate limit, it will reset after 60 seconds.
* Typically, complete responses are available 0-15 seconds after session creation.
* The live pricing API is not CORS-friendly.
* Prices may vary in different countries on the same itinerary (usually for legal or business reasons).

#### Valid Inputs

**Session Creation (POST)**

* `Content-Type` header: `application/x-www-form-urlencoded` or `application/json`.
	Ensure your payload matches i.e. it is form-encoded or a JSON object.
* For a one-way trip, do not supply the `inboundDate`.
* Dates must be in `yyyy-mm-dd` from today (UTC) until a year from now.
* Always set `groupPricing` to true. This flag exists for backwards compatibility only.
* Always set `locationSchema` to `iata` for ease of use.
* References: [currencies], [locales], and [markets] (countries).
* Pax limits: `adults` 1-8, `children` 0-8, `infants` not greater than `adults`.
* Cabin class: `Economy`, `PremiumEconomy`, `Business`, `First`.
* Origin/destination inputs may be the following codes that represent airports or cities:

| Type   |      Example      |  API input | What this means |
|----------|-------------|------|------|
| IATA code |  SFO | `SFO-iata` | San Francisco International Airport |
| Skyscanner code |    NYCA   |   `NYCA-sky` | All airports in New York City |
| Latitude and longitude | 37.6786° N, 122.4528° W |    `37.678,-122.452-latlong` | The nearest airport to this lat/long pair |
| IP address | 188.39.95.93 |    `188.39.95.93-ip` | The nearest airport by IP geolocation |
 
For help finding the right code, refer to the Location Autosuggest section in the [full documentation]. We have an autosuggest service to help you find locations.

**Polling (GET) query string options**

* `pageindex=0&pagesize=20` gives the first page of 20 results.
* `sortorder=asc&sorttype=price` gives the itineraries in ascending order of price.
* `stops=0` gives non-stop itineraries only.
* `duration=300` gives itineraries no longer than 300 minutes (5 hours).
* Further filtering options are documented in the [full documentation].

#### Debugging

We strongly recommend you download a tool like Fiddler or Charles and use it with the [test harness]. This allows you to see what correct communication with the API looks like, thereby helping you to debug your application.

#### Known Gotchas

* Got a `403` error? Did you remember to append your `apiKey` to the query string?
* You will receive a `500` error if your session creation `Content-Type` does not match the request payload.
* Some itineraries have segments that require you to change airport during your journey. You may occasionally observe missing values in the `Segments` array due to this. Similar edge-case behaviour may be seen in segments that are refuel-only stops (these are exceptionally rare these days).

  [full documentation]: <http://business.skyscanner.net/portal/en-GB/Documentation/ApiOverview>
  [currencies]: <http://business.skyscanner.net/portal/en-GB/Documentation/Currencies>
  [locales]: <http://business.skyscanner.net/portal/en-GB/Documentation/Locales>
  [markets]: <http://business.skyscanner.net/portal/en-GB/Documentation/Markets>
  [test harness]: <http://business.skyscanner.net/portal/en-GB/Documentation/FlightsLivePricingQuickStart>
  [sample_live_flights_pricing_response.json]: <https://github.com/Skyscanner/api-documentation/live_flights_pricing/sample_live_flights_pricing_response.json>