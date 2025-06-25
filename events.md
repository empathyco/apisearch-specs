# Apisearch events

You can catch these events in your frame by checking the name

```javascript

window.addEventListener("message", function(event) {
  const data = event.data;
  if (data.name === "apisearch_item_was_clicked") {
    // Do something
  }
}
```

Device values can be <code>desktop</code>, <code>phone</code> and <code>tablet</code>.

### apisearch_search

Each time a new search is computed, this event is thrown. This event is not exactly the used one in our servers, as we work with complex graph algorithms to ensure that searches are built properly, discarding all intermediate and useless words. In this case, we consider a new word when the final user stops typing during 2 seconds. Then, the current word is considered a search

```javascript
{
  name: "apisearch_search",
  query_text: string,
  query: Query,
  device: string,
  site: string
{
```

### apisearch_result_items

When the search layer renders new items, this event is dispatched with these items. Important. These items inside the event have been rendered, but can be still hidden.

```javascript
{
  name: "apisearch_result_items",
  query: Query,
  query_text: string,
  with_results: boolean,
  page: integer,
  items: array[],
  device: string,
  site: string
{
```

### apisearch_item_was_clicked

Each time an item is clicked and notified to our servers.

```javascript
{
  name: "apisearch_item_was_clicked",
  item_id: string,
  query: array,
  result: array,
  device: string,
  site: string
}
```

### apisearch_item_was_interacted

Each time an items is interacted and notified to our servers. When an items is clicked, is notified as interaction as well.

```javascript
{
  name: "apisearch_item_was_interacted",
  item_id: string,
  query: array,
  result: array,
  device: string,
  site: string
}
```

### apisearch_purchase_was_done

Each time a new purchase is notified to our servers

```javascript
{
  name: "apisearch_purchase_was_done",
  site: string,
  device: string
}
```

### show_apisearch_iframe

When the user opens the search layer

### close_apisearch_iframe

When the user closes the search layer
