---
description: >-
  You can easily create licenses through dashboard but it will be most of the
  times done using the web API, when your customer makes the purchase.
---

# Creating Licenses

## Creating a License

A license inherits all it's properties from the license policy attached to it's product. So a license can be created by hitting the license API endpoint with an empty body - `{}` too.

But you may way want to override some properties like `allowedActivations`, `validity`, add some `metadata` etc. You can check out all the properties in web API [reference page](https://api.cryptlex.com/v3/docs#operation/V3LicensesPost). Following is a sample request which you will usually make to create a license:

{% api-method method="post" host="https://api.cryptlex.com" path="/v3/licenses" %}
{% api-method-summary %}
Creating License
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer access token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="validity" type="number" required=false %}
The duration  after which the license will expire.
{% endapi-method-parameter %}

{% api-method-parameter name="allowedActivations" type="number" required=false %}
Allowed number of activations for the license.
{% endapi-method-parameter %}

{% api-method-parameter name="userId" type="string" required=false %}
Unique identifier for the user.
{% endapi-method-parameter %}

{% api-method-parameter name="productId" type="string" required=true %}
Unique identifier for the product.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "key": "0A2035-E8A2A3-4D31B7-8FF9C6-81A6CA-539E54",
  "revoked": false,
  "suspended": false,
  "totalActivations": 0,
  "totalDeactivations": 0,
  "validity": 2595000,
  "expirationStrategy": "immediate",
  "fingerprintMatchingStrategy": "fuzzy",
  "allowedActivations": 1,
  "allowedDeactivations": 10,
  "type": "node-locked",
  "allowedFloatingClients": 0,
  "serverSyncGracePeriod": 2595000,
  "serverSyncInterval": 3600,
  "leaseDuration": 0,
  "expiresAt": "2018-06-06T08:49:17.9361143Z",
  "allowVmActivation": true,
  "userLocked": false,
  "productId": "63dfd63e-ed71-4f84-9236-02ee0ddb062c",
  "user": null,
  "allowedCountries": [],
  "disallowedCountries": [],
  "allowedIpAddresses": [],
  "disallowedIpAddresses": [],
  "metadata": [],
  "tags": [],
  "id": "23d9646c-34f5-4d37-adeb-f7f77b927bdb",
  "createdAt": "2018-05-06T08:49:17.9361143Z",
  "updatedAt": "2018-05-06T08:49:17.9361158Z"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}
