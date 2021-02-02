---
description: For checking the transaction status
---

# Transaction Requery

{% api-method method="get" host="https://secure.riipay.my" path="/v1/query/transaction" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="merchant\_code" type="string" required=true %}
You merchant code
{% endapi-method-parameter %}

{% api-method-parameter name="transaction\_reference" type="string" required=true %}
Riipay Transaction Reference
{% endapi-method-parameter %}

{% api-method-parameter name="signature" type="string" required=true %}
Electronic signature. See Query Signature
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Query Signature

In order to ensure the data integrity of information passing to Riipay, we use electronic signature for validation and verification. The hash algorithm used is **`md5`**.

The signature value is a concatenated string consists of the following fields, in the following order:

* Merchant Code \(provided by Riipay\)
* Secret Key \(provided by Riipay\)
* Transaction Reference \(response or return by Riipay\)

```php
public function generateSignature()
{
    $hash = $merchant_code . $secret_key . $transaction_reference;
    $signature = md5($hash);
    return $signature;
}
```

## Query Response

Please refer to [Payment Response](payment-request.md#payment-response), the same response format in **JSON** format will be returned

## Rate Limit

Please take note that all query endpoint will have rate limit, where you can only query maximum 2 times per second

