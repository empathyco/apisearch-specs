# Custom Prices Endpoint Specification

This document outlines the specification for a **Custom Prices Strategy** within Apisearch. A custom prices strategy is required when Apisearch receives a page of results, and due to the complexity of pricing (e.g., customer-specific prices or advanced pricing calculations), an intermediate server-side call is needed before displaying final product prices.

By following this specification, the custom prices strategy can be seamlessly integrated with Apisearch, enabling dynamic and flexible pricing for complex use cases.

## Endpoint Overview

The integration requires a server-side endpoint that accepts a single query parameter named **`ids`**, which contains a comma-separated list of product IDs. The endpoint returns a JSON response where each product ID maps to its corresponding pricing details.

### Request Example

```http
GET https://endpoint.com/path/?ids=1,2,3,4
```

### Response Example

The response is a JSON object. Each product ID from the query parameter maps to an object containing its pricing information:

```json
{
    "1": {
        "p": 12,
        "p_c": "12 EUR",
        "op": 15,
        "op_c": "15 EUR",
        "wd": true,
        "dp": 15
    },
    "2": {
        "p": 30,
        "p_c": "30 USD",
        "wd": false
    }
}
```

### Response Object Structure

Each product object includes the following fields:

* `p` (float): Current price of the product.
* `p_c` (string): Current price of the product including the currency.
* `op` (float, optional): Original price before discount (if applicable).
* `op_c` (string, optional): Original price before discount including the currency (if applicable).
* `wd` (boolean): Indicates whether the product has an active discount (`true` or `false`).
* `dp` (integer, optional): Discount percentage, included only if the product has an active discount.

#### Example Field Behavior

* A product with a discount:

```json
{
    "p": 12,
    "p_c": "12 EUR",
    "op": 15,
    "op_c": "15 EUR",
    "wd": true,
    "dp": 15
}
```

* A product without a discount

```json
{
    "p": 30,
    "p_c": "30 USD",
    "wd": false
}
```

### Notes and Recommendations
* **Efficiency**:
  The endpoint must be highly optimized, as it will be called every time new products are rendered. Minimize latency and ensure scalability to handle frequent requests.
* **Optional Fields**
  Fields `op`, `op_c`, and `dp` are included only for products with an active discount (`wd` set to `true`).

> [!WARNING]  
> Any delay in this endpoint could negatively impact page load times and user experience.
