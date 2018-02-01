--- 

title: MithrilX 

language_tabs: 
   - shell 

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a> 

includes: 
   - errors 

search: true 

--- 

# Introduction 



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
 

**Version:** v1 

# /V1/INDICES
## ***GET*** 

**Summary:** Returns a list of indices

### HTTP Request 
`***GET*** /v1/indices` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |
| decorate | query | Decorations (computed fields) to be attached to the returned objects. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of indices |

## ***POST*** 

**Summary:** Creates a new index

### HTTP Request 
`***POST*** /v1/indices` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | The newly created index |
| 400 |  - Max number of constituents condition failed. - There are no pairs that match given conditions. - One or multiple "pair_ids" are not consistent with "quote_currency_id", ie. are not quotable in that currency - The "quote_currency_id" doesn't map to a "quotable" currency - You did not provide a definition for a "dynamic" index - You provided a definition for a "static" index  |
| 409 |  - An index with the same "code" already exists  |

# /V1/INDICES/{ID_OR_CODE}
## ***GET*** 

**Summary:** Gets an index by id or code

### HTTP Request 
`***GET*** /v1/indices/{id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding index |
| 404 |  - No corresponding index found for this id or code  |

# /V1/INDICES/TEST
## ***POST*** 

**Summary:** Test dynamic index definition and return suitable constituents if test was successful

### HTTP Request 
`***POST*** /v1/indices/test` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of suitable constituents (pairs) |
| 400 |  - NO_CONSTITUENTS_HISTORY_DATA - NO_CONSTITUENTS_MATCH - INDEX_CODE_ALREADY_EXIST - INDEX_NAME_ALREADY_EXIST  |

# /V1/INDICES/{ID_OR_CODE}/HISTORY
## ***GET*** 

**Summary:** Return history data for index constituents

### HTTP Request 
`***GET*** /v1/indices/{id_or_code}/history` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |
| from | query | Start date of index pairs historic data. Format is UTC+0 timestamp. | No |  |
| to | query | End date of index pairs historic data. Format is UTC+0 timestamp. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A history data for index constituents |

# /V1/INDICES/{ID_OR_CODE}/REBALANCE
## ***POST*** 

**Summary:** Perform index rebalancing - recalculate constituents percents and use it from rebalance moment to calculate index prices

### HTTP Request 
`***POST*** /v1/indices/{id_or_code}/rebalance` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The index decorated with rebalanced index pairs |
| 404 |  - No corresponding index found for this id  |

# /V1/INDICES/{ID_OR_CODE}/PAIRS/{PAIR_ID_OR_CODE}
## ***POST*** 

**Summary:** Add given pair to index constituents

### HTTP Request 
`***POST*** /v1/indices/{id_or_code}/pairs/{pair_id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The index with added pair |
| 404 |  - No corresponding index found for this id  |

## ***DELETE*** 

**Summary:** Delete given pair from index constituents

### HTTP Request 
`***DELETE*** /v1/indices/{id_or_code}/pairs/{pair_id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The index with deleted pair |
| 404 |  - No corresponding index found for this id  |

# /V1/INDICES/{ID_OR_CODE}/MODIFICATIONS
## ***GET*** 

**Summary:** Find potential index constituents modifications

### HTTP Request 
`***GET*** /v1/indices/{id_or_code}/modifications` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Potential index constituents modifications (additions and deletions) |

# /V1/EXCHANGE-PAIRS
## ***GET*** 

**Summary:** Returns a list of exchange pairs

### HTTP Request 
`***GET*** /v1/exchange-pairs` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |
| id | query | id | No |  |
| id__in | query | id__in | No |  |
| code | query | The resource's unique code | No |  |
| code__in | query | code__in | No |  |
| pair_id | query | pair_id | No |  |
| pair_id__in | query | pair_id__in | No |  |
| pair_code | query | The resource's unique code | No |  |
| pair_code__in | query | pair_code__in | No |  |
| decorate | query | Decorations (computed fields) to be attached to the returned objects. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of exchange pairs |

# /V1/EXCHANGE-PAIRS/{ID_OR_CODE}
## ***GET*** 

**Summary:** Gets a exchange pair by id or code

### HTTP Request 
`***GET*** /v1/exchange-pairs/{id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding exchange pair |
| 404 |  - No exchange pair was found for this id or code  |

# /V1/PAIRS
## ***GET*** 

**Summary:** Returns a list of pairs

### HTTP Request 
`***GET*** /v1/pairs` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |
| base_currency_id | query | base_currency_id | No |  |
| base_currency_id__in | query | base_currency_id__in | No |  |
| base_currency_code | query | The resource's unique code | No |  |
| base_currency_code__in | query | base_currency_code__in | No |  |
| quote_currency_id | query | quote_currency_id | No |  |
| quote_currency_id__in | query | quote_currency_id__in | No |  |
| quote_currency_code | query | The resource's unique code | No |  |
| quote_currency_code__in | query | quote_currency_code__in | No |  |
| id | query | id | No |  |
| id__in | query | id__in | No |  |
| code | query | The resource's unique code | No |  |
| code__in | query | code__in | No |  |
| decorate | query | Decorations (computed fields) to be attached to the returned objects. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of pairs |

# /V1/PAIRS/{ID_OR_CODE}
## ***GET*** 

**Summary:** Gets a pair by id

### HTTP Request 
`***GET*** /v1/pairs/{id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding pair |
| 404 |  - No pair was found for this id or code  |

# /V1/PAIRS/{ID_OR_CODE}/HISTORY
## ***GET*** 

**Summary:** Return history data for True Price constituents

### HTTP Request 
`***GET*** /v1/pairs/{id_or_code}/history` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |
| from | query | Start date of Trie Price pairs historic data. Format is UTC+0 timestamp. | No |  |
| to | query | End date of True Price pairs historic data. Format is UTC+0 timestamp. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A history data for True Price constituents |

# /V1/PAIRS/{ID_OR_CODE}/VOLUME_RATIO
## ***GET*** 

**Summary:** Return history daily rolling sum volume ratio for TruePrice pair

### HTTP Request 
`***GET*** /v1/pairs/{id_or_code}/volume_ratio` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |
| from | query | Start date of historic data. Format is UTC+0 timestamp. | No |  |
| to | query | End date of historic data. Format is UTC+0 timestamp. | No |  |
| time_frame | query | time_frame | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A  history daily rolling sum volume ratio for TruePrice pair |

# /V1/EXCHANGES
## ***GET*** 

**Summary:** Returns a list of exchanges

### HTTP Request 
`***GET*** /v1/exchanges` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of exchanges |

# /V1/EXCHANGES/{ID_OR_CODE}
## ***GET*** 

**Summary:** Gets an exchange by id or code

### HTTP Request 
`***GET*** /v1/exchanges/{id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding exchange |
| 404 |  - No corresponding exchange found  |

# /V1/CURRENCIES
## ***GET*** 

**Summary:** Returns a list of currencies

### HTTP Request 
`***GET*** /v1/currencies` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |
| id | query | id | No |  |
| id__in | query | id__in | No |  |
| code | query | The resource's unique code | No |  |
| code__in | query | code__in | No |  |
| type | query | type | No |  |
| type__in | query | type__in | No |  |
| quotable | query | Whether or not the currency can act act as a quote currency within. See the quote_currency_id property on pairs and indices. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of currencies |

# /V1/CURRENCIES/{ID_OR_CODE}
## ***GET*** 

**Summary:** Gets a currency by id or code

### HTTP Request 
`***GET*** /v1/currencies/{id_or_code}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding currency |
| 404 |  - No currency corresponds to this id or code  |

# /V1/CURRENCIES/{ID_OR_CODE}/VOLUME_RATIO
## ***GET*** 

**Summary:** Return history daily rolling sum volume ratio for given currency

### HTTP Request 
`***GET*** /v1/currencies/{id_or_code}/volume_ratio` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |
| from | query | Start date of historic data. Format is UTC+0 timestamp. | No |  |
| to | query | End date of historic data. Format is UTC+0 timestamp. | No |  |
| time_frame | query | time_frame | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A  history daily rolling sum volume ratio for given currency |

# /V1/CURRENCIES/{ID_OR_CODE}/SUPPLIES
## ***GET*** 

**Summary:** Return history monthly supply snapshots for given currency

### HTTP Request 
`***GET*** /v1/currencies/{id_or_code}/supplies` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |
| from | query | Start date of historic data. Format is UTC+0 timestamp. | No |  |
| to | query | End date of historic data. Format is UTC+0 timestamp. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A history monthly supply snapshots for given currency |

# /V1/USERS
## ***GET*** 

**Summary:** Returns a list of users

### HTTP Request 
`***GET*** /v1/users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| limit | query | The maximum amount of resources to be returned for this request. | No |  |
| offset | query | The amount of resources skipped for this request. | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A list of users |

## ***POST*** 

**Summary:** Create a new user

### HTTP Request 
`***POST*** /v1/users` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | The newly created index |

# /V1/USERS/CURRENT
## ***GET*** 

**Summary:** Get a current user by provided authorization data

### HTTP Request 
`***GET*** /v1/users/current` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The current user |

# /V1/USERS/{ID_OR_MAIL}
## ***GET*** 

**Summary:** Get a user by id or email

### HTTP Request 
`***GET*** /v1/users/{id_or_mail}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The corresponding user |

# /V1/USERS/LOGIN
## ***POST*** 

**Summary:** Verify user credentials and return user data with JWT token if credentials are valid

### HTTP Request 
`***POST*** /v1/users/login` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | The user data with JWT token |

# /V1/USERS/{ID}
## ***DELETE*** 

**Summary:** Delete existing user

### HTTP Request 
`***DELETE*** /v1/users/{id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | The resource's identifier | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 202 | Accepted when successfully deleted |

# /V1/USERS/{ID_OR_MAIL}/SUBSCRIPTIONS
## ***GET*** 

**Summary:** Get email subscriptions for given user

### HTTP Request 
`***GET*** /v1/users/{id_or_mail}/subscriptions` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A  email subscriptions for given user |

## ***POST*** 

**Summary:** Subscribe given user to receive TruePrice and composite indices email notifications

### HTTP Request 
`***POST*** /v1/users/{id_or_mail}/subscriptions` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | A  email subscriptions for given user |

## ***DELETE*** 

**Summary:** Unsubscribe given user to receive TruePrice and composite indices email notifications

### HTTP Request 
`***DELETE*** /v1/users/{id_or_mail}/subscriptions` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 202 | Accepted when successfully unsubscribed |

# /V1/USERS/FEEDBACK
## ***POST*** 

**Summary:** Send feedback message to MithrilX team

### HTTP Request 
`***POST*** /v1/users/feedback` 

**Responses**

| Code | Description |
| ---- | ----------- |
| 202 | Accepted when message successfully sent |

# /V1/USERS/{ID_OR_MAIL}/NEWSLETTER
## ***POST*** 

**Summary:** Subscribe user to MithrilX Newsletter

### HTTP Request 
`***POST*** /v1/users/{id_or_mail}/newsletter` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id_or_code | path | id_or_code | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 202 | Accepted when user successfully subscribed |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
