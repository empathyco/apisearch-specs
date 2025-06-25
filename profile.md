# Query

This is a simple Apisearch reduced query. Taken in consideration that the whole profile will be stored in our servers,
the query itself doesn't have information about the aggregations, but only about the query text, the filters and the
desired order. This will allow us to reduce the query complexity, as well as the centralization of the configuration
under the Apisearch infrastructure.

```json
{
  "site": "es",
  "q": "Shoes",
  "price": {
    "from": 10,
    "to": 30
  },
  "category": ["12", "14"],
  "brand": "My brand",
  "sort": {
    "field": "category",
    "order": "asc"
  },
  "page": 1,
  "count": 20
}
```

Site value is only **required** when the index is defined as multisite.

As you can see in this first query example, the query text is defined with the key "q" and the order with the key
"sort". Both are optional. The order field can handle both `asc` and `desc` orders, except for the `score` field, that
will ignore the order (as will always be descendant results).

Parameter `count` is optional, as per default will consider the one defined in the profile.

After defining all these parameters (`site`, `q`, `sort`, `page` and `count`), all other keys will be considered filters and
will produce aggregations for the response object. If the profile does not contain this filter definition, the filter
will be applied anyway, but no aggregations will be returned. If the filtered field does not exist, then an exception
will be thrown and an HTTP 400 response error will be returned.

## HTTP layer

In order to perform an Apisearch query, you need these defined parameters

- `app_uuid` - This will define the identification of the user under Apisearch infrastructure.
- `index_uuid` - This will identify your index under your account
- `token_uuid` - Private query token generated in our infrastructure

Our endpoints are distributed per zones. In the next example, we assume the zone `eu1`, as well as the API version `v1`.
This could change in your case. In this case, we are querying the index `ce2847ff` under the APP `6acd40f4f8d3`, using
the private token `b2e6e33592a7`. Values lengths are not real and may vary in your case.

We are performing the query against the main profile.

```shell
curl "https://eu1.apisearch.cloud/v1/6acd40f4f8d3/indices/ce2847ff/profile?token=b2e6e33592a7"
 -H "Content-Type: application/json"
 -d`
  {
    "site": "es",
    "q": "Shoes",
    "category": ["12", "14"],
    "page": "2"
  }
  `
```

# Result

This is the received result from the Apisearch servers. As you can see, this is a reduced perspective of an item, as the
ID is the only value returned by Apisearch. Result is returned as a JSON object.

```json
{
  "items": ["1", "2", "10", "100"],
  "n": "1000",
  "aggregations": {
    "category": {
      "14": 110,
      "78": 40,
      "79": 13
    },
    "brand": {
      "Another brand": 10,
      "Yet another brand": 3
    }
  }
}
```

Each aggregation returns an object, having the value of the aggregation as key (can be an ID or a text), and the number
of occurrences as value.

# Result with alternatives

When a more than 2 words query is done and the system does not found any result, Apisearch will try to perform partial
queries in order to find good results by deleting one or more words from the original query. 

Taking an example when we search `trousers with flowers` and we get 0 results.

```json
{
  "items": [],
  "alternatives": {
    "trousers": {
      "formatted_query": "trousers <del>with flowers</del>",
      "items": ["12", "13", "14"],
      "n": 13
    },
    "with flowers": {
      "formatted_query": "<del>trousers</del> with flowers",
      "items": ["20", "21"],
      "n": 2
    }
  }
}
```

Each alternative value will give the total number of results with that transformed query, as well as an HTML ready
formatted query with the rectification. Result items are properly sorted given a standard score order.

