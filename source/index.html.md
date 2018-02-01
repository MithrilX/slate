---
title: MithrilX v1
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - javascript--nodejs: Node.JS
  - ruby: Ruby
  - python: Python
  - java: Java
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2


---


<h1 id="MithrilX">MithrilX v1</h1>


> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.


## REST API


This documentation presents the general guidelines followed when designing this API. For specific information about a particular endpoint, please refer to the endpoint's documentation itself.


### Authentication


Endpoints are all public for now.


### Headers


Most endpoints require the `Content-Type` and `Accept` headers to be set to `application/json`.


### Response formats


#### POST requests
- `data`: Holds the object(s) created in this request


#### GET requests
- `data`: Holds the object(s) returned for this request
- `meta`: In case multiple objects are being returned, or if the endpoint is paginated, the `meta` object will hold the following properties
    - `limit`: The limit used to query the endpoint
    - `offset`: The amount of objects skipped in the query
    - `count`: The total amount of objects corresponding to the query (they might not all be returned due to `limit`)


#### PATCH requests
- `meta`: Holds a `count` property corresponding to the amount of objects impacted by the update


#### DELETE requests
- `meta`: Holds a `count` property corresponding to the amount of objects deleted by the request


#### Errors
- `error`: Simple boolean indicating that the request failed, always `true`
- `message`: A human readable message indicating the reason why the request failed
- `meta`: Various information about the error, including a JSON `schema` property if a payload was the cause of the error


### Keywords


The API makes use of a few keyword, to be used in the query string parameters
- `limit`: For GET / requests, allows you to control how many objects are being returned
- `offset`: For GET / requests, allows use to skip objects from the research
- `decorate`: Is an array of computed properties which you wish to see included for each object. For performance reasons, those computed are not returned by default
- `expand`: NOT AVAILABLE YET


### Filters & Operators


GET / requests might expose a number of filters in order to refine the search you're making.


Additionally, some of those filters might also support operators. Operators are a convenient way to fine tune your query and are appended to the property name using `__`.


Existing operators:
- `in`: Similar to SQL's IN clause
- `nin`: Similar to SQL's NOT IN clause
- `ne`: Different than
- `gt`: Greater than
- `gte`: Greater than equal
- `lt`: Lower than
- `lte`: Lower than equal


Example filter coupled with an operator : `type__in`


## WebSockets Feed


### Subscribing/Unsubscribing


Subscribing is done by sending a `subscribe` event and unsubscribing is done by sending an `unsubscribe` event. Both payloads follow the same format.


You can choose to subscribe to *indices*, *pairs* or *exchange pairs* changes. Currently only the `ticker` channel is available.


#### Example:
```javascript
socket.send({
  "type": "subscribe",
  "index_ids": [1, 2],
  "pair_ids": [2, 3],
  "exchange_pair_ids": [1, 5],
  "channels": ["ticker"]
})
```


### Events


#### `ticker:index`


This event is triggered for each change on an index.


```
{
  "type": "ticker:index",
  "data": {
      "index_id": 1,
      "price": "34563546",
      "change_hour": "-0.035",
      "change_24h": "1.123",
      "change_week": "0.897",
      "change_month": "1.453",
      "change_three_months": "5.453",
      "change_year": "10.453"
  }
}
```


#### `ticker:pair`


This event is triggered for each change on a pair.


```
{
  "type": "ticker:pair",
  "data": {
      "pair_id": 1,
      "price": "34563546",
      "volume": "23434",
      "change_hour": "-0.035",
      "change_24h": "1.123",
      "change_week": "0.897",
      "change_month": "1.453",
      "change_three_months": "5.453",
      "change_year": "10.453"
  }
}
```


#### `ticker:exchange_pair`


This event is triggered for each change on a exchange pair.


```
{
  "type": "ticker:exchange_pair",
  "data": {
      "pair_id": 1,
      "price": "34563546",
      "volume": "23434",
      "change_hour": "-0.035",
      "change_24h": "1.123",
      "change_week": "0.897",
      "change_month": "1.453",
      "change_three_months": "5.453",
      "change_year": "10.453"
  }
}
```


#### `error`


Error can occur if you send a wrong subscription for example.


```
{
  "type": "error",
  "message": "Something went wrong."
}
```


Base URLs:


* <a href="https://api-mithril-x-staging.herokuapp.com">https://api-mithril-x-staging.herokuapp.com</a>


* <a href="wss://feed-mithril-x-staging.herokuapp.com">wss://feed-mithril-x-staging.herokuapp.com</a>


<h1 id="MithrilX-Default">Default</h1>


## get__v1_indices


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/indices \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/indices HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/indices',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/indices',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/indices',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/indices', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/indices");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/indices`


*Returns a list of indices*


<h3 id="get__v1_indices-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|integer|false|The maximum amount of resources to be returned for this request.|
|offset|query|integer|false|The amount of resources skipped for this request.|
|decorate|query|array[string]|false|Decorations (computed fields) to be attached to the returned objects.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|decorate|latest_price|
|decorate|definition|
|decorate|pair_ids|
|decorate|pairs|
|decorate|history_change|


> Example responses


```json
{
  "meta": {
    "limit": 20,
    "offset": 0,
    "count": 0
  },
  "data": [
    {
      "id": 1,
      "name": "string",
      "code": "string",
      "weighting_method": "market",
      "definition_type": "static",
      "quote_currency_id": 1,
      "start_date": 946659600000,
      "start_value": 1,
      "definition": {
        "min_market_cap_usd": "string",
        "max_market_cap_usd": "string",
        "min_daily_volume_ratio": 0,
        "min_public_float": 0,
        "max_currencies": 0,
        "mineable": true,
        "exchange_ids": [
          1
        ]
      },
      "pair_ids": [
        1
      ],
      "pairs": [
        {
          "pair_id": 1,
          "price": "string",
          "percent": "string",
          "target_percent": "string",
          "latest_market_cap": "string",
          "month_daily_volume_ratio": "string"
        }
      ],
      "latest_price": "string",
      "change_hour": "string",
      "change_24h": "string",
      "change_week": "string",
      "change_month": "string",
      "change_three_months": "string",
      "change_year": "string"
    }
  ]
}
```


<h3 id="get__v1_indices-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list of indices|Inline|


<h3 id="get__v1_indices-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» meta|object|false|No description|
|»» limit|integer|true|The maximum amount of resources to be returned for this request.|
|»» offset|integer|true|The amount of resources skipped for this request.|
|»» count|integer|true|The total amount of resources corresponding to this research.|
|» data|[object]|false|No description|
|»» id|integer|true|No description|
|»» name|string|true|Reference name for this index.|
|»» code|string|true|Unique identifier for this index.|
|»» weighting_method|string|true|Defines how the constituants of this index are combined in order to calculate it's price. See http://www.investopedia.com/terms/e/equalweight.asp and http://www.investopedia.com/terms/m/marketweight.asp for a detailed explanation.|
|»» definition_type|string|true|Whether this index is dynamic - is initially based on suggestions - or static - defined by a fixed list of currencies. To be noted that suggestions for dynamic indices are only suggestions and don't affect the index's composition over time.|
|»» quote_currency_id|integer|true|The currency in which this index is quoted. The currency must be quotable.|
|»» start_date|integer|true|The starting date from which the price of index is calculated. Format is UTC+0 timestamp.|
|»» start_value|integer|true|The initial value of the index. It's an integer and it's usually a very simple number to start with like 1000|
|»» definition|object|false|List of filters defining how a dynamic index's suggestions are made.|
|»»» min_market_cap_usd|string|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» max_market_cap_usd|string|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» min_daily_volume_ratio|number|false|Leave null to ignore. Min of volume/supply sum by last 30 days, index will only contain pairs which match this filter.)|
|»»» min_public_float|number|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter. Value from 0 to 1.)|
|»»» max_currencies|integer|false|Leave null to ignore. Maximum amount of currencies to be suggested for the index.|
|»»» mineable|boolean|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» exchange_ids|[integer]|false|Leave null to ignore. The list of exchanges included in this index.|
|»» pair_ids|[integer]|false|The ids of the pairs constituting this index.|
|»» pairs|[object]|false|The pairs constituting this index.|
|»»» pair_id|integer|false|The True Price pair ID.|
|»»» price|string|false|The price of the pair in index measure. It is not equal pair True Price.|
|»»» percent|string|false|The current percent (initial or after last rebalansing) of that pair in index.|
|»»» target_percent|string|false|The target percent (at the current moment) of that pair in index.|
|»»» latest_market_cap|string|false|Latest pair market cap.|
|»»» month_daily_volume_ratio|string|false|Average daily volume ratio (daily volume/total supply) for the last 30 days.|
|»» latest_price|string|false|The latest known price for this index. Computed, obtained by decoration.|
|»» change_hour|string|false|Price change percent compare to hour ago.|
|»» change_24h|string|false|Price change percent compare to 24 hour ago.|
|»» change_week|string|false|Price change percent compare to week ago.|
|»» change_month|string|false|Price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_indices_{id_or_code}


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code} \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code} HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/indices/{id_or_code}`


*Gets an index by id or code*


<h3 id="get__v1_indices_{id_or_code}-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|array[string]|true|id_or_code|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|latest_price|
|id_or_code|definition|
|id_or_code|pair_ids|
|id_or_code|pairs|
|id_or_code|history_change|


> Example responses


```json
{
  "data": {
    "id": 1,
    "name": "string",
    "code": "string",
    "weighting_method": "market",
    "definition_type": "static",
    "quote_currency_id": 1,
    "start_date": 946659600000,
    "start_value": 1,
    "definition": {
      "min_market_cap_usd": "string",
      "max_market_cap_usd": "string",
      "min_daily_volume_ratio": 0,
      "min_public_float": 0,
      "max_currencies": 0,
      "mineable": true,
      "exchange_ids": [
        1
      ]
    },
    "pair_ids": [
      1
    ],
    "pairs": [
      {
        "pair_id": 1,
        "price": "string",
        "percent": "string",
        "target_percent": "string",
        "latest_market_cap": "string",
        "month_daily_volume_ratio": "string"
      }
    ],
    "latest_price": "string",
    "change_hour": "string",
    "change_24h": "string",
    "change_week": "string",
    "change_month": "string",
    "change_three_months": "string",
    "change_year": "string"
  }
}
```
```json
{
  "error": true,
  "message": "string",
  "meta": {}
}
```


<h3 id="get__v1_indices_{id_or_code}-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The corresponding index|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|- No corresponding index found for this id or code|Inline|


<h3 id="get__v1_indices_{id_or_code}-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|Main entity of the MithrilX system, indices group crypto currencies the same way the S&P500 groups the US 500 top companies.|
|»» id|integer|true|No description|
|»» name|string|true|Reference name for this index.|
|»» code|string|true|Unique identifier for this index.|
|»» weighting_method|string|true|Defines how the constituants of this index are combined in order to calculate it's price. See http://www.investopedia.com/terms/e/equalweight.asp and http://www.investopedia.com/terms/m/marketweight.asp for a detailed explanation.|
|»» definition_type|string|true|Whether this index is dynamic - is initially based on suggestions - or static - defined by a fixed list of currencies. To be noted that suggestions for dynamic indices are only suggestions and don't affect the index's composition over time.|
|»» quote_currency_id|integer|true|The currency in which this index is quoted. The currency must be quotable.|
|»» start_date|integer|true|The starting date from which the price of index is calculated. Format is UTC+0 timestamp.|
|»» start_value|integer|true|The initial value of the index. It's an integer and it's usually a very simple number to start with like 1000|
|»» definition|object|false|List of filters defining how a dynamic index's suggestions are made.|
|»»» min_market_cap_usd|string|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» max_market_cap_usd|string|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» min_daily_volume_ratio|number|false|Leave null to ignore. Min of volume/supply sum by last 30 days, index will only contain pairs which match this filter.)|
|»»» min_public_float|number|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter. Value from 0 to 1.)|
|»»» max_currencies|integer|false|Leave null to ignore. Maximum amount of currencies to be suggested for the index.|
|»»» mineable|boolean|false|Leave null to ignore. Suggestions for the index will only contain pairs which match this filter.|
|»»» exchange_ids|[integer]|false|Leave null to ignore. The list of exchanges included in this index.|
|»» pair_ids|[integer]|false|The ids of the pairs constituting this index.|
|»» pairs|[object]|false|The pairs constituting this index.|
|»»» pair_id|integer|false|The True Price pair ID.|
|»»» price|string|false|The price of the pair in index measure. It is not equal pair True Price.|
|»»» percent|string|false|The current percent (initial or after last rebalansing) of that pair in index.|
|»»» target_percent|string|false|The target percent (at the current moment) of that pair in index.|
|»»» latest_market_cap|string|false|Latest pair market cap.|
|»»» month_daily_volume_ratio|string|false|Average daily volume ratio (daily volume/total supply) for the last 30 days.|
|»» latest_price|string|false|The latest known price for this index. Computed, obtained by decoration.|
|»» change_hour|string|false|Price change percent compare to hour ago.|
|»» change_24h|string|false|Price change percent compare to 24 hour ago.|
|»» change_week|string|false|Price change percent compare to week ago.|
|»» change_month|string|false|Price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


Status Code **404**


|Name|Type|Required|Description|
|---|---|---|---|
|» error|boolean|false|No description|
|» message|string|false|No description|
|» meta|object|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_indices_{id_or_code}_history


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/history");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/indices/{id_or_code}/history`


*Return history data for index constituents*


<h3 id="get__v1_indices_{id_or_code}_history-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|string|true|id_or_code|
|from|query|integer|false|Start date of index pairs historic data. Format is UTC+0 timestamp.|
|to|query|integer|false|End date of index pairs historic data. Format is UTC+0 timestamp.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|minute|
|id_or_code|hour|
|id_or_code|day|
|id_or_code|week|
|id_or_code|month|


> Example responses


```json
{
  "history_prices": [
    {
      "timestamp": 0,
      "price": "string",
      "pairs": [
        {
          "pair_id": 1,
          "price": "string"
        }
      ]
    }
  ],
  "rebalance_points": [
    {
      "timestamp": 0,
      "initial": true,
      "active": true,
      "addition_pair_ids": [
        1
      ],
      "deletion_pair_ids": [
        1
      ],
      "percents": [
        {
          "pair_id": 1,
          "percent": "string"
        }
      ]
    }
  ]
}
```


<h3 id="get__v1_indices_{id_or_code}_history-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A history data for index constituents|Inline|


<h3 id="get__v1_indices_{id_or_code}_history-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» history_prices|[object]|false|No description|
|»» timestamp|integer|false|Timestamp of index pair historic data. Format is UTC+0 timestamp.|
|»» price|string|false|The price of the index at the given time.|
|»» pairs|[object]|false|No description|
|»»» pair_id|integer|false|The True Price pair ID.|
|»»» price|string|false|The price of the pair in index measure. It is not equal pair True Price.|
|»» rebalance_points|[object]|false|No description|
|»»» timestamp|integer|false|Timestamp of index pair historic data. Format is UTC+0 timestamp.|
|»»» initial|boolean|false|True if it is initial percents calculated on index start date|
|»»» active|boolean|false|True if it is latest initial or rebalance point that used to calculate prices now|
|»»» addition_pair_ids|[integer]|false|No description|
|»»» deletion_pair_ids|[integer]|false|No description|
|»»» percents|[object]|false|No description|
|»»»» pair_id|integer|false|The True Price pair ID.|
|»»»» percent|string|false|The current percent (initial or after last rebalansing) of that pair in index.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_indices_{id_or_code}_modifications


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/indices/{id_or_code}/modifications");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/indices/{id_or_code}/modifications`


*Find potential index constituents modifications*


<h3 id="get__v1_indices_{id_or_code}_modifications-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


```json
{
  "additions": [
    {
      "pair_id": 1,
      "since": 0,
      "days": 0,
      "is_allowed": true
    }
  ],
  "deletions": [
    {
      "pair_id": 1,
      "since": 0,
      "days": 0,
      "is_allowed": true
    }
  ]
}
```


<h3 id="get__v1_indices_{id_or_code}_modifications-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Potential index constituents modifications (additions and deletions)|Inline|


<h3 id="get__v1_indices_{id_or_code}_modifications-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» additions|[object]|false|No description|
|»» pair_id|integer|false|The True Price pair ID.|
|»» since|integer|false|Timestamp since given modification exist. Format is UTC+0 timestamp.|
|»» days|integer|false|No description|
|»» is_allowed|boolean|false|No description|
|» deletions|[object]|false|No description|
|»» pair_id|integer|false|The True Price pair ID.|
|»» since|integer|false|Timestamp since given modification exist. Format is UTC+0 timestamp.|
|»» days|integer|false|No description|
|»» is_allowed|boolean|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_exchange-pairs


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/exchange-pairs`


*Returns a list of exchange pairs*


<h3 id="get__v1_exchange-pairs-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|integer|false|The maximum amount of resources to be returned for this request.|
|offset|query|integer|false|The amount of resources skipped for this request.|
|id|query|integer|false|id|
|id__in|query|array[integer]|false|id__in|
|code|query|string|false|The resource's unique code|
|code__in|query|array[string]|false|code__in|
|pair_id|query|integer|false|pair_id|
|pair_id__in|query|array[integer]|false|pair_id__in|
|pair_code|query|string|false|The resource's unique code|
|pair_code__in|query|array[string]|false|pair_code__in|
|decorate|query|array[string]|false|Decorations (computed fields) to be attached to the returned objects.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|decorate|latest_price|
|decorate|latest_volume|
|decorate|history_change|


> Example responses


```json
{
  "meta": {
    "limit": 20,
    "offset": 0,
    "count": 0
  },
  "data": [
    {
      "id": 1,
      "pair_id": 1,
      "exchange_id": 1,
      "cross_currency_id": 1,
      "code": "string",
      "pair_code": "string",
      "exchange_code": "string",
      "cross_currency_code": "string",
      "latest_price": "string",
      "latest_volume": "string",
      "change_hour": "string",
      "change_24h": "string",
      "change_week": "string",
      "change_month": "string",
      "change_three_months": "string",
      "change_year": "string"
    }
  ]
}
```


<h3 id="get__v1_exchange-pairs-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list of exchange pairs|Inline|


<h3 id="get__v1_exchange-pairs-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» meta|object|false|No description|
|»» limit|integer|true|The maximum amount of resources to be returned for this request.|
|»» offset|integer|true|The amount of resources skipped for this request.|
|»» count|integer|true|The total amount of resources corresponding to this research.|
|» data|[object]|false|No description|
|»» id|integer|true|No description|
|»» pair_id|integer|true|The base pair for this exchange pair.|
|»» exchange_id|integer|false|The exchange that produce the pair.|
|»» cross_currency_id|integer|true|The cross currency used in this exchange pair.|
|»» code|string|false|The resource's unique code|
|»» pair_code|string|false|The resource's unique code|
|»» exchange_code|string|false|The resource's unique code|
|»» cross_currency_code|string|false|The resource's unique code|
|»» latest_price|string|false|The latest known price for this exchange pair.|
|»» latest_volume|string|false|The latest known volume for this exchange pair.|
|»» change_hour|string|false|Exchange pair price change percent compare to hour ago.|
|»» change_24h|string|false|Exchange pair price change percent compare to 24 hour ago.|
|»» change_week|string|false|Exchange pair price change percent compare to week ago.|
|»» change_month|string|false|Exchange pair price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_exchange-pairs_{id_or_code}


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code} \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code} HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code}',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code}',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code}',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code}', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/exchange-pairs/{id_or_code}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/exchange-pairs/{id_or_code}`


*Gets a exchange pair by id or code*


<h3 id="get__v1_exchange-pairs_{id_or_code}-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|array[string]|true|id_or_code|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|latest_price|
|id_or_code|latest_volume|
|id_or_code|history_change|


> Example responses


```json
{
  "data": {
    "id": 1,
    "pair_id": 1,
    "exchange_id": 1,
    "cross_currency_id": 1,
    "code": "string",
    "pair_code": "string",
    "exchange_code": "string",
    "cross_currency_code": "string",
    "latest_price": "string",
    "latest_volume": "string",
    "change_hour": "string",
    "change_24h": "string",
    "change_week": "string",
    "change_month": "string",
    "change_three_months": "string",
    "change_year": "string"
  }
}
```
```json
{
  "error": true,
  "message": "string",
  "meta": {}
}
```


<h3 id="get__v1_exchange-pairs_{id_or_code}-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The corresponding exchange pair|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|- No exchange pair was found for this id or code|Inline|


<h3 id="get__v1_exchange-pairs_{id_or_code}-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» pair_id|integer|false|The base pair for this exchange pair.|
|»» exchange_id|integer|false|The exchange that produce the pair.|
|»» cross_currency_id|integer|false|The cross currency used in this exchange pair.|
|»» code|string|false|The resource's unique code|
|»» pair_code|string|false|The resource's unique code|
|»» exchange_code|string|false|The resource's unique code|
|»» cross_currency_code|string|false|The resource's unique code|
|»» latest_price|string|false|The latest known price for this exchange pair.|
|»» latest_volume|string|false|The latest known volume for this exchange pair.|
|»» change_hour|string|false|Exchange pair price change percent compare to hour ago.|
|»» change_24h|string|false|Exchange pair price change percent compare to 24 hour ago.|
|»» change_week|string|false|Exchange pair price change percent compare to week ago.|
|»» change_month|string|false|Exchange pair price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


Status Code **404**


|Name|Type|Required|Description|
|---|---|---|---|
|» error|boolean|false|No description|
|» message|string|false|No description|
|» meta|object|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_pairs


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/pairs \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/pairs HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/pairs',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/pairs',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/pairs',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/pairs', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/pairs");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/pairs`


*Returns a list of pairs*


<h3 id="get__v1_pairs-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|integer|false|The maximum amount of resources to be returned for this request.|
|offset|query|integer|false|The amount of resources skipped for this request.|
|base_currency_id|query|integer|false|base_currency_id|
|base_currency_id__in|query|array[integer]|false|base_currency_id__in|
|base_currency_code|query|string|false|The resource's unique code|
|base_currency_code__in|query|array[string]|false|base_currency_code__in|
|quote_currency_id|query|integer|false|quote_currency_id|
|quote_currency_id__in|query|array[integer]|false|quote_currency_id__in|
|quote_currency_code|query|string|false|The resource's unique code|
|quote_currency_code__in|query|array[string]|false|quote_currency_code__in|
|id|query|integer|false|id|
|id__in|query|array[integer]|false|id__in|
|code|query|string|false|The resource's unique code|
|code__in|query|array[string]|false|code__in|
|decorate|query|array[string]|false|Decorations (computed fields) to be attached to the returned objects.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|decorate|latest_price|
|decorate|latest_volume|
|decorate|history_change|


> Example responses


```json
{
  "meta": {
    "limit": 100,
    "offset": 0,
    "count": 0
  },
  "data": [
    {
      "id": 1,
      "base_currency_id": 1,
      "quote_currency_id": 1,
      "code": "string",
      "base_currency_code": "string",
      "quote_currency_code": "string",
      "latest_price": "string",
      "latest_volume": "string",
      "change_hour": "string",
      "change_24h": "string",
      "change_week": "string",
      "change_month": "string",
      "change_three_months": "string",
      "change_year": "string"
    }
  ]
}
```


<h3 id="get__v1_pairs-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list of pairs|Inline|


<h3 id="get__v1_pairs-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» meta|object|false|No description|
|»» limit|integer|true|The maximum amount of resources to be returned for this request.|
|»» offset|integer|true|The amount of resources skipped for this request.|
|»» count|integer|true|The total amount of resources corresponding to this research.|
|» data|[object]|false|No description|
|»» id|integer|true|No description|
|»» base_currency_id|integer|true|The base currency for this pair.|
|»» quote_currency_id|integer|true|The currency in which this pair is quoted.|
|»» code|string|false|The resource's unique code|
|»» base_currency_code|string|false|The resource's unique code|
|»» quote_currency_code|string|false|The resource's unique code|
|»» latest_price|string|false|The latest known price for this pair.|
|»» latest_volume|string|false|The latest known volume for this pair.|
|»» change_hour|string|false|Price change percent compare to hour ago.|
|»» change_24h|string|false|Price change percent compare to 24 hour ago.|
|»» change_week|string|false|Price change percent compare to week ago.|
|»» change_month|string|false|Price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_pairs_{id_or_code}


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code} \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code} HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/pairs/{id_or_code}`


*Gets a pair by id*


<h3 id="get__v1_pairs_{id_or_code}-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|array[string]|true|id_or_code|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|latest_price|
|id_or_code|latest_volume|
|id_or_code|history_change|


> Example responses


```json
{
  "data": {
    "id": 1,
    "base_currency_id": 1,
    "quote_currency_id": 1,
    "code": "string",
    "base_currency_code": "string",
    "quote_currency_code": "string",
    "latest_price": "string",
    "latest_volume": "string",
    "change_hour": "string",
    "change_24h": "string",
    "change_week": "string",
    "change_month": "string",
    "change_three_months": "string",
    "change_year": "string"
  }
}
```
```json
{
  "error": true,
  "message": "string",
  "meta": {}
}
```


<h3 id="get__v1_pairs_{id_or_code}-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The corresponding pair|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|- No pair was found for this id or code|Inline|


<h3 id="get__v1_pairs_{id_or_code}-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» base_currency_id|integer|false|The base currency for this pair.|
|»» quote_currency_id|integer|false|The currency in which this pair is quoted.|
|»» code|string|false|The resource's unique code|
|»» base_currency_code|string|false|The resource's unique code|
|»» quote_currency_code|string|false|The resource's unique code|
|»» latest_price|string|false|The latest known price for this pair.|
|»» latest_volume|string|false|The latest known volume for this pair.|
|»» change_hour|string|false|Price change percent compare to hour ago.|
|»» change_24h|string|false|Price change percent compare to 24 hour ago.|
|»» change_week|string|false|Price change percent compare to week ago.|
|»» change_month|string|false|Price change percent compare to month ago.|
|»» change_three_months|string|false|Price change percent compare to three month ago.|
|»» change_year|string|false|Price change percent compare to year ago.|


Status Code **404**


|Name|Type|Required|Description|
|---|---|---|---|
|» error|boolean|false|No description|
|» message|string|false|No description|
|» meta|object|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_pairs_{id_or_code}_history


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/history");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/pairs/{id_or_code}/history`


*Return history data for True Price constituents*


<h3 id="get__v1_pairs_{id_or_code}_history-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|string|true|id_or_code|
|from|query|integer|false|Start date of True Price pairs historic data. Format is UTC+0 timestamp.|
|to|query|integer|false|End date of True Price pairs historic data. Format is UTC+0 timestamp.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|minute|
|id_or_code|hour|
|id_or_code|day|
|id_or_code|week|
|id_or_code|month|


> Example responses


```json
[
  {
    "timestamp": 0,
    "price": "string",
    "exchange_pairs": [
      {
        "exchange_pair_id": 1,
        "exchange_id": 1,
        "price": "string",
        "volume": "string"
      }
    ]
  }
]
```


<h3 id="get__v1_pairs_{id_or_code}_history-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A history data for True Price constituents|Inline|


<h3 id="get__v1_pairs_{id_or_code}_history-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» timestamp|integer|false|Timestamp of True Price pair historic data. Format is UTC+0 timestamp.|
|» price|string|false|The price of the True Price at the given time.|
|» exchange_pairs|[object]|false|No description|
|»» exchange_pair_id|integer|false|No description|
|»» exchange_id|integer|false|The exchange that produce the pair.|
|»» price|string|false|The exchange pair close price at the given time.|
|»» volume|string|false|The exchange pair volume for the given time frame.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_pairs_{id_or_code}_volume_ratio


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/pairs/{id_or_code}/volume_ratio");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/pairs/{id_or_code}/volume_ratio`


*Return history daily rolling sum volume ratio for TruePrice pair*


<h3 id="get__v1_pairs_{id_or_code}_volume_ratio-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|integer|true|id_or_code|
|from|query|integer|false|Start date of historic data. Format is UTC+0 timestamp.|
|to|query|integer|false|End date of historic data. Format is UTC+0 timestamp.|
|time_frame|query|string|false|time_frame|


#### Enumerated Values


|Parameter|Value|
|---|---|
|time_frame|minute|
|time_frame|hour|
|time_frame|day|
|time_frame|week|
|time_frame|month|


> Example responses


```json
[
  {
    "timestamp": 0,
    "volume_ratio": "string"
  }
]
```


<h3 id="get__v1_pairs_{id_or_code}_volume_ratio-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A  history daily rolling sum volume ratio for TruePrice pair|Inline|


<h3 id="get__v1_pairs_{id_or_code}_volume_ratio-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» timestamp|integer|false|Timestamp of historic data. Format is UTC+0 timestamp.|
|» volume_ratio|string|false|The rolling sum volume ratio (volume/supply) for the given day.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_exchanges


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/exchanges \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/exchanges HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/exchanges',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/exchanges',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/exchanges',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/exchanges', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/exchanges");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/exchanges`


*Returns a list of exchanges*


<h3 id="get__v1_exchanges-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|integer|false|The maximum amount of resources to be returned for this request.|
|offset|query|integer|false|The amount of resources skipped for this request.|


> Example responses


```json
{
  "meta": {
    "limit": 20,
    "offset": 0,
    "count": 0
  },
  "data": [
    {
      "id": 1,
      "name": "string",
      "code": "string",
      "website": "string"
    }
  ]
}
```


<h3 id="get__v1_exchanges-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list of exchanges|Inline|


<h3 id="get__v1_exchanges-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» meta|object|false|No description|
|»» limit|integer|true|The maximum amount of resources to be returned for this request.|
|»» offset|integer|true|The amount of resources skipped for this request.|
|»» count|integer|true|The total amount of resources corresponding to this research.|
|» data|[object]|false|No description|
|»» id|integer|true|No description|
|»» name|string|true|Reference name for this exchange.|
|»» code|string|true|Unique identifier for this exchange.|
|»» website|string|true|The exchange's website.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_exchanges_{id_or_code}


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code} \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code} HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code}',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code}',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code}',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code}', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/exchanges/{id_or_code}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/exchanges/{id_or_code}`


*Gets an exchange by id or code*


<h3 id="get__v1_exchanges_{id_or_code}-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


```json
{
  "data": {
    "id": 1,
    "name": "string",
    "code": "string",
    "website": "string"
  }
}
```
```json
{
  "error": true,
  "message": "string",
  "meta": {}
}
```


<h3 id="get__v1_exchanges_{id_or_code}-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The corresponding exchange|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|- No corresponding exchange found|Inline|


<h3 id="get__v1_exchanges_{id_or_code}-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» name|string|false|Reference name for this exchange.|
|»» code|string|false|Unique identifier for this exchange.|
|»» website|string|false|The exchange's website.|


Status Code **404**


|Name|Type|Required|Description|
|---|---|---|---|
|» error|boolean|false|No description|
|» message|string|false|No description|
|» meta|object|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_currencies


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/currencies \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/currencies HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/currencies',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/currencies',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/currencies',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/currencies', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/currencies");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/currencies`


*Returns a list of currencies*


<h3 id="get__v1_currencies-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|integer|false|The maximum amount of resources to be returned for this request.|
|offset|query|integer|false|The amount of resources skipped for this request.|
|id|query|integer|false|id|
|id__in|query|array[integer]|false|id__in|
|code|query|string|false|The resource's unique code|
|code__in|query|array[string]|false|code__in|
|type|query|string|false|type|
|type__in|query|array[string]|false|type__in|
|quotable|query|boolean|false|Whether or not the currency can act act as a quote currency within. See the quote_currency_id property on pairs and indices.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|type|crypto|
|type|fiat|
|type__in|crypto|
|type__in|fiat|


> Example responses


```json
{
  "meta": {
    "limit": 100,
    "offset": 0,
    "count": 0
  },
  "data": [
    {
      "id": 1,
      "name": "string",
      "code": "string",
      "type": "crypto",
      "web_site": "string",
      "description": "string",
      "decimals": 0,
      "quotable": true,
      "supply": "string",
      "total_supply": "string",
      "max_supply": "string",
      "last_24h_volume_usd": "string",
      "symbol": "string",
      "mineable": true,
      "true_price": true
    }
  ]
}
```


<h3 id="get__v1_currencies-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A list of currencies|Inline|


<h3 id="get__v1_currencies-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» meta|object|false|No description|
|»» limit|integer|true|The maximum amount of resources to be returned for this request.|
|»» offset|integer|true|The amount of resources skipped for this request.|
|»» count|integer|true|The total amount of resources corresponding to this research.|
|» data|[object]|false|No description|
|»» id|integer|true|No description|
|»» name|string|true|Reference name for this currency.|
|»» code|string|true|Unique identifier for this currency.|
|»» type|string|true|Whether the currency is a crypto currency or a fiat/regular one.|
|»» web_site|string|false|The currency web-site.|
|»» description|string|false|The description of this currency.|
|»» decimals|integer|true|The amount of decimals the currency accepts. Needed to parse the zero-decimals numbers.|
|»» quotable|boolean|true|Whether or not the currency can act act as a quote currency within. See the quote_currency_id property on pairs and indices.|
|»» supply|string|true|The circulating supply, total amount of coins available for this currency.|
|»» total_supply|string|false|The total supply for this currency (not null only if total supply not equals circulating supply).|
|»» max_supply|string|false|The maximum reachable supply for this currency.|
|»» last_24h_volume_usd|string|false|The latest 24h USD volume.|
|»» symbol|string|true|Unique, graphic symbol commonly used for this currency. Not always available.|
|»» mineable|boolean|true|Whether the currency is mineable or not.|
|»» true_price|boolean|false|Whether the currency has True Price indices.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_currencies_{id_or_code}


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code} \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code} HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/currencies/{id_or_code}`


*Gets a currency by id or code*


<h3 id="get__v1_currencies_{id_or_code}-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


```json
{
  "data": {
    "id": 1,
    "name": "string",
    "code": "string",
    "type": "crypto",
    "web_site": "string",
    "description": "string",
    "decimals": 0,
    "quotable": true,
    "supply": "string",
    "total_supply": "string",
    "max_supply": "string",
    "last_24h_volume_usd": "string",
    "symbol": "string",
    "mineable": true,
    "true_price": true
  }
}
```
```json
{
  "error": true,
  "message": "string",
  "meta": {}
}
```


<h3 id="get__v1_currencies_{id_or_code}-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The corresponding currency|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|- No currency corresponds to this id or code|Inline|


<h3 id="get__v1_currencies_{id_or_code}-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» name|string|false|Reference name for this currency.|
|»» code|string|false|Unique identifier for this currency.|
|»» type|string|false|Whether the currency is a crypto currency or a fiat/regular one.|
|»» web_site|string|false|The currency web-site.|
|»» description|string|false|The description of this currency.|
|»» decimals|integer|false|The amount of decimals the currency accepts. Needed to parse the zero-decimals numbers.|
|»» quotable|boolean|false|Whether or not the currency can act act as a quote currency within. See the quote_currency_id property on pairs and indices.|
|»» supply|string|false|The circulating supply, total amount of coins available for this currency.|
|»» total_supply|string|false|The total supply for this currency (not null only if total supply not equals circulating supply).|
|»» max_supply|string|false|The maximum reachable supply for this currency.|
|»» last_24h_volume_usd|string|false|The latest 24h USD volume.|
|»» symbol|string|false|Unique, graphic symbol commonly used for this currency. Not always available.|
|»» mineable|boolean|false|Whether the currency is mineable or not.|
|»» true_price|boolean|false|Whether the currency has True Price indices.|


Status Code **404**


|Name|Type|Required|Description|
|---|---|---|---|
|» error|boolean|false|No description|
|» message|string|false|No description|
|» meta|object|false|No description|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_currencies_{id_or_code}_volume_ratio


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/volume_ratio");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/currencies/{id_or_code}/volume_ratio`


*Return history daily rolling sum volume ratio for given currency*


<h3 id="get__v1_currencies_{id_or_code}_volume_ratio-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|integer|true|id_or_code|
|from|query|integer|false|Start date of historic data. Format is UTC+0 timestamp.|
|to|query|integer|false|End date of historic data. Format is UTC+0 timestamp.|
|time_frame|query|string|false|time_frame|


#### Enumerated Values


|Parameter|Value|
|---|---|
|time_frame|minute|
|time_frame|hour|
|time_frame|day|
|time_frame|week|
|time_frame|month|


> Example responses


```json
[
  {
    "timestamp": 0,
    "volume_ratio": "string"
  }
]
```


<h3 id="get__v1_currencies_{id_or_code}_volume_ratio-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A  history daily rolling sum volume ratio for given currency|Inline|


<h3 id="get__v1_currencies_{id_or_code}_volume_ratio-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» timestamp|integer|false|Timestamp of historic data. Format is UTC+0 timestamp.|
|» volume_ratio|string|false|The rolling sum volume ratio (volume/supply) for the given day.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_currencies_{id_or_code}_supplies


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/currencies/{id_or_code}/supplies");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/currencies/{id_or_code}/supplies`


*Return history monthly supply snapshots for given currency*


<h3 id="get__v1_currencies_{id_or_code}_supplies-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|string|true|id_or_code|
|from|query|integer|false|Start date of historic data. Format is UTC+0 timestamp.|
|to|query|integer|false|End date of historic data. Format is UTC+0 timestamp.|


#### Enumerated Values


|Parameter|Value|
|---|---|
|id_or_code|minute|
|id_or_code|hour|
|id_or_code|day|
|id_or_code|week|
|id_or_code|month|


> Example responses


```json
[
  {
    "timestamp": 0,
    "supply": "string"
  }
]
```


<h3 id="get__v1_currencies_{id_or_code}_supplies-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A history monthly supply snapshots for given currency|Inline|


<h3 id="get__v1_currencies_{id_or_code}_supplies-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» timestamp|integer|false|Timestamp of historic data. Format is UTC+0 timestamp.|
|» supply|string|false|The historic supply snapshot.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_users_current


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/users/current \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/users/current HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/current',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/current',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/users/current',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/users/current', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/current");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/users/current`


*Get a current user by provided authorization data*


> Example responses


```json
{
  "data": {
    "id": 1,
    "email": "string",
    "role": "consumer",
    "login": "string",
    "token": "string",
    "password": "string"
  }
}
```


<h3 id="get__v1_users_current-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The current user|Inline|


<h3 id="get__v1_users_current-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» email|string|false|The user email address.|
|»» role|string|false|The user role one of "admin" or "consumer".|
|»» login|string|false|The login of the user, can be empty for consumer role users.|
|»» token|string|false|The security token that can be used to perform authorized requests.|
|»» password|string|false|The user password, can be specified when register new user. Never returned.|


<aside class="success">
This operation does not require authentication
</aside>


## post__v1_users_login


> Code samples


```shell
# You can also use wget
curl -X POST https://api-mithril-x-staging.herokuapp.com/v1/users/login \
  -H 'Accept: application/json'


```


```http
POST https://api-mithril-x-staging.herokuapp.com/v1/users/login HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/login',
  method: 'post',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/login',
{
  method: 'POST',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.post 'https://api-mithril-x-staging.herokuapp.com/v1/users/login',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.post('https://api-mithril-x-staging.herokuapp.com/v1/users/login', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/login");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`POST /v1/users/login`


*Verify user credentials and return user data with JWT token if credentials are valid*


> Example responses


```json
{
  "data": {
    "id": 1,
    "email": "string",
    "role": "consumer",
    "login": "string",
    "token": "string",
    "password": "string"
  }
}
```


<h3 id="post__v1_users_login-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The user data with JWT token|Inline|


<h3 id="post__v1_users_login-responseschema">Response Schema</h3>


Status Code **200**


|Name|Type|Required|Description|
|---|---|---|---|
|» data|object|false|No description|
|»» id|integer|false|No description|
|»» email|string|false|The user email address.|
|»» role|string|false|The user role one of "admin" or "consumer".|
|»» login|string|false|The login of the user, can be empty for consumer role users.|
|»» token|string|false|The security token that can be used to perform authorized requests.|
|»» password|string|false|The user password, can be specified when register new user. Never returned.|


<aside class="success">
This operation does not require authentication
</aside>


## get__v1_users_{id_or_mail}_subscriptions


> Code samples


```shell
# You can also use wget
curl -X GET https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions \
  -H 'Accept: application/json'


```


```http
GET https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  method: 'get',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
{
  method: 'GET',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.get 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.get('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`GET /v1/users/{id_or_mail}/subscriptions`


*Get email subscriptions for given user*


<h3 id="get__v1_users_{id_or_mail}_subscriptions-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


<h3 id="get__v1_users_{id_or_mail}_subscriptions-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A  email subscriptions for given user|Inline|


<aside class="success">
This operation does not require authentication
</aside>


## post__v1_users_{id_or_mail}_subscriptions


> Code samples


```shell
# You can also use wget
curl -X POST https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions \
  -H 'Accept: application/json'


```


```http
POST https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  method: 'post',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
{
  method: 'POST',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.post 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.post('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`POST /v1/users/{id_or_mail}/subscriptions`


*Subscribe given user to receive TruePrice and composite indices email notifications*


<h3 id="post__v1_users_{id_or_mail}_subscriptions-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


<h3 id="post__v1_users_{id_or_mail}_subscriptions-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A  email subscriptions for given user|Inline|


<aside class="success">
This operation does not require authentication
</aside>


## delete__v1_users_{id_or_mail}_subscriptions


> Code samples


```shell
# You can also use wget
curl -X DELETE https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions \
  -H 'Accept: application/json'


```


```http
DELETE https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  method: 'delete',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
{
  method: 'DELETE',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.delete 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.delete('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/subscriptions");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`DELETE /v1/users/{id_or_mail}/subscriptions`


*Unsubscribe given user to receive TruePrice and composite indices email notifications*


<h3 id="delete__v1_users_{id_or_mail}_subscriptions-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


<h3 id="delete__v1_users_{id_or_mail}_subscriptions-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|Accepted when successfully unsubscribed|Inline|


<aside class="success">
This operation does not require authentication
</aside>


## post__v1_users_feedback


> Code samples


```shell
# You can also use wget
curl -X POST https://api-mithril-x-staging.herokuapp.com/v1/users/feedback \
  -H 'Accept: application/json'


```


```http
POST https://api-mithril-x-staging.herokuapp.com/v1/users/feedback HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/feedback',
  method: 'post',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/feedback',
{
  method: 'POST',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.post 'https://api-mithril-x-staging.herokuapp.com/v1/users/feedback',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.post('https://api-mithril-x-staging.herokuapp.com/v1/users/feedback', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/feedback");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`POST /v1/users/feedback`


*Send feedback message to MithrilX team*


> Example responses


<h3 id="post__v1_users_feedback-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|Accepted when message successfully sent|Inline|


<aside class="success">
This operation does not require authentication
</aside>


## post__v1_users_{id_or_mail}_newsletter


> Code samples


```shell
# You can also use wget
curl -X POST https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter \
  -H 'Accept: application/json'


```


```http
POST https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter HTTP/1.1
Host: api-mithril-x-staging.herokuapp.com


Accept: application/json


```


```javascript
var headers = {
  'Accept':'application/json'


};


$.ajax({
  url: 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter',
  method: 'post',


  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})


```


```javascript--nodejs
const request = require('node-fetch');


const headers = {
  'Accept':'application/json'


};


fetch('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter',
{
  method: 'POST',


  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});


```


```ruby
require 'rest-client'
require 'json'


headers = {
  'Accept' => 'application/json'
}


result = RestClient.post 'https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter',
  params: {
  }, headers: headers


p JSON.parse(result)


```


```python
import requests
headers = {
  'Accept': 'application/json'
}


r = requests.post('https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter', params={


}, headers = headers)


print r.json()


```


```java
URL obj = new URL("https://api-mithril-x-staging.herokuapp.com/v1/users/{id_or_mail}/newsletter");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());


```


`POST /v1/users/{id_or_mail}/newsletter`


*Subscribe user to MithrilX Newsletter*


<h3 id="post__v1_users_{id_or_mail}_newsletter-parameters">Parameters</h3>


|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id_or_code|path|any|true|id_or_code|


> Example responses


<h3 id="post__v1_users_{id_or_mail}_newsletter-responses">Responses</h3>


|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|Accepted when user successfully subscribed|Inline|


<aside class="success">
This operation does not require authentication
</aside>


