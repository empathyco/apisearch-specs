## Simple Site feed

This is the Apisearch item structure.

```json
{
    "id": "ID",
    "ti": "Title",
    "d": "Description",
    "b": "Brand",
    "p": "Price, 9.34 (float)",
    "op": "Old price, 13.47 (float)",
    "p_c": "Price with currency, 9.34€ (string)",
    "op_c": "Price with currency, 13.47€ (string)",
    "av": "Product is available (bool)",
    "e": "Ean",
    "sku": "Sku",
    "u": "Url",
    "i": "Image",
    "wv": "Product has variants (boolean)",
    "iv": "Product is a variant (boolean)",
    "tg": ["tag1", "tag2", "tag3"],
    "c": "Category",
    "c1": "Category level 1",
    "c2": "Category level 2",
    "c3": "Category level 3",
    "c4": "Category level 4",
    "c5": "Category level 5",
    "opt": "Options",
    "s": "Sales (integer)",
    "pr": "Priority 1-10"
    "rs": "Review stars (integer)",
    "rc": "Number of reviews (integer)"
}
```

All values can have one or several elements. For example, you could provide a list of same-level categories by doing this```json

```json
{
    "id": "ID",
    "c": ["category1", "category2", "categoryN"],
}
```

> Diferent level categories are meant for different filters. Same filter values means same field.

Here some explanation about some fields

- Sales (s) is a normalized representation of your sales. Normalization means that should be a value between 0 and 1000, for example, where 1000 is the most sold item. This number can be changed in order to obfuscate the real number.
- Priority (pr) is a normalized representation of the priority of your product. Most important products for abstract queries should have higher values here.

## Multisite feed

For multisite, you can work with both sites and languages. A site can hold a
language, and a language can be held by multiple sites. Fields `ID`,`title`,
`url` and `image` are required. Site and language names have no standard,
so you can choose your own codes.

```json
{
    "id": "ID",
    "ti": "Title",
    "u": "Url",
    "i": "Image",
    "languages": {
        "ca": {
            "ti": "Title in catalan",
            "d": "Description in catalan"
        },
        "es": {
            "ti": "Title in spanish",
            "d": "Description in spanish"
        }
    },
    "sites": {
        "site_in_cat": {
            "language": "ca",
            "p": 1.32,
            "p_c": "1.32 €",
            "op": 2.01,
            "op_c": "2.01 €"
        },
        "site_in_esp": {
            "language": "es",
            "p": 4.32,
            "p_c": "USD 4.32",
            "op": 5.01,
            "op_c": "USD 5.01"
        }
    }
}
```

Using languages is not a must, and should be used when some sites share the same
language. The main goal for languages is to reduce both the feed and the index
sizes.

```json
{
    "id": "ID",
    "ti": "Title",
    "u": "Url",
    "i": "Image",
    "sites": {
        "site_in_cat": {
            "ti": "Title in catalan",
            "d": "Description in catalan",
            "language": "ca",
            "p": 1.32,
            "p_c": "1.32 €",
            "op": 2.01,
            "op_c": "2.01 €"
        },
        "site_in_esp": {
            "ti": "Title in spanish",
            "d": "Description in spanish",
            "p": 4.32,
            "p_c": "USD 4.32",
            "op": 5.01,
            "op_c": "USD 5.01"
        }
    }
}
```

## B2B prices per group

You can assign a different price for each of your user groups. The default prices in the root of the item will still be used if, on search time, the group is not found.

```json
{
    "id": "ID",
    "ti": "Title",
    "u": "Url",
    "i": "Image",
    "users": {
        "group_1": {
            "p": 10,
            "pc": "10 EUR",
            "op": "15",
            "opc": "15 EUR,
            "av": true,
        },
        "group_2": {
            "p": 13,
            "pc": "13 EUR",
            "op": "19",
            "opc": "19 EUR,
            "av": true
        },
        "group_3": {
            "av": false
        }
    }
}
```

Inside each group you can find all prices data. By default, available is `true` even if the group is not defined. If the group is defined with the availability boolean as `false`, then this object will not be found per this group.

```json
{
    "p": "Price, 9.34 (float)",
    "op": "Old price, 13.47 (float)",
    "p_c": "Price with currency, 9.34€ (string)",
    "op_c": "Price with currency, 13.47€ (string)",
    "av": "Product is available for this group (bool)",
}
```


## Deletion lines

Deletions must be specified here as well by using the same format, but with a specific flag instead of the whole item body. Lines can be mixed.

```jsonl
{"id": "ID1", "delete": true}
{"id": "ID2", "delete": true}
{"id": "ID3", "delete": true}
```

## Output format (JSONL)

The feed will be served in [`JSONL`](https://jsonlines.org) format. Each line must represent one item formated as JSON. Each line **must ends with a break line**. This means that break lines inside the content, like titles or descriptions, must be deleted.

```jsonl
{"id": "ID1", "ti": "Title1"}
{"id": "ID2", "ti": "Title2"}
{"id": "ID3", "ti": "Title3"}
```
