# Skyscanner API Token Service

Use this API to encrypt your API key for use in client-side session polling. You should not contact the token service endpoint from your client-side code directly, but instead proxy such a request through your back-end.

#### Prerequisites

* A good understanding of the Skyscanner Flights API.

#### Example Request and Response

``` http
GET https://api.skyscanner.net/apiservices/token/v2/gettoken?apikey=YOUR_API_KEY_HERE HTTP/1.1
Host: api.skyscanner.net
Accept: application/json
```

``` http
HTTP/1.1 200 OK
Date: Tue, 15 Sep 2015 13:45:33 GMT
Content-Type: application/json

"json_string_containing_token"
```

The token is valid for 30 minutes and can be used wherever you are required to supply an `apiKey`.