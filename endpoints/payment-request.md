# Payment Request

{% swagger baseUrl="https://secure.riipay.my" path="/v1/payment" method="get" summary="Initiate a payment request" %}
{% swagger-description %}
\** For testing, please use 

`https://secure.uat.riipay.my`

\




\


This endpoint allows you to initial a new payment request.
{% endswagger-description %}

{% swagger-parameter in="query" name="merchant_code" type="string" %}
Your merchant code
{% endswagger-parameter %}

{% swagger-parameter in="query" name="reference" type="string" %}
Your order unique reference number. Can be order ID
{% endswagger-parameter %}

{% swagger-parameter in="query" name="description" type="string" %}
Your order description
{% endswagger-parameter %}

{% swagger-parameter in="query" name="currency_code" type="string" %}
Your order currency code e.g. 

**MYR**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="amount" type="number" %}
Total order amount. Format:

\


2 decimals without thousand separator.

\


eg: 12.00 or 12345.60

\


Minimum 1.00.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="signature" type="string" %}
Electronic signature. see Request Signature
{% endswagger-parameter %}

{% swagger-parameter in="query" name="customer_name" type="string" %}
Customer name
{% endswagger-parameter %}

{% swagger-parameter in="query" name="customer_email" type="string" %}
Customer email address
{% endswagger-parameter %}

{% swagger-parameter in="query" name="customer_phone" type="string" %}
Customer phone number
{% endswagger-parameter %}

{% swagger-parameter in="query" name="customer_ip" type="string" %}
Customer IP Address
{% endswagger-parameter %}

{% swagger-parameter in="query" name="return_url" type="string" %}
Return URL. Submit this field if you wish to override your default return URL in dashboard profile settings.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="callback_url" type="string" %}
Callback URL. Submit this field if you wish to override your default callback URL in dashboard profile settings.Yo {
{% endswagger-parameter %}

{% swagger-response status="200" description="You will be redirected to payment page." %}
```
```
{% endswagger-response %}
{% endswagger %}

![Your customer will be redirect to Riipay secure payment portal](../.gitbook/assets/selection\_914.png)

## Request Signature

In order to ensure the data integrity of information passing to Riipay, we use electronic signature for validation and verification. The hash algorithm used is **`md5`**.

The signature value is a concatenated string consists of the following fields, in the following order:

* merchant\_code (provided by Riipay)
* Secret Key (provided by Riipay)
* reference
* currency\_code
* amount

{% hint style="info" %}
amount have to be a string of 2 decimals without thousand separator. e.g.: 1234.00
{% endhint %}

```php
public function generateSignature($order)
{
    //convert order total to a string of 2 decimals with no thousand separator
    $amount = number_format($order->total, 2, '.', '');
    $hash = $merchant_code . $secret_key . $order->reference . $order->currency_code . $amount;
    $signature = md5($hash);
    return $signature;
}
```

## Sample Request

Assume that you receive a new order, and the details are as follows:

| Field           | Value                                                  |
| --------------- | ------------------------------------------------------ |
| merchant\_code  | TEST                                                   |
| reference       | SO20201109-01                                          |
| description     | Order SO20201109-01: 1 Adidas Sneakers                 |
| currency\_code  | MYR                                                    |
| amount          | 1234.00                                                |
| customer\_name  | Mr. Lee                                                |
| customer\_email | lee@testing.com                                        |
| customer\_phone | 0123456789                                             |
| customer\_ip    | 123.321.12.123                                         |
| signature       | __[_See below._](payment-request.md#request-signature) |

### Generate Signature Value

{% hint style="info" %}
Assume the "Secret Key" = "a1b2c3d4e5f6"
{% endhint %}

```php
$amount = number_format(1234.00, 2, '.', '');
$hash = 'TEST' . 'a1b2c3d4e5f6' . 'SO20201109-01' . 'MYR' . $amount;
$signature = md5($hash);
```

{% hint style="success" %}
**Signature Output**

759c1d9805ba0f4bf624098a36258cb3
{% endhint %}

### Send GET request to Riipay

```php
{{BASE_URL}}/v1/payment?merchant_code=TEST&reference=SO20201109-01&description=Order+SO20201109-01%3A+1+
Adidas+Sneakers&currency_code=MYR&=amount=1234.00&customer_name=Mr.+Lee&customer_email=lee%40testing.com&customer_phone=01234567
89&customer_ip=123.321.12.123&signature=759c1d9805ba0f4bf624098a36258cb3
```

## Payment Response

You will receive payment response from Riipay after payment requested or processed.

There are 2 response types, one is **Client-to-Server Return** (Return URL), another is **Server-to-Server Callback** (Callback URL).\
\
For **Client-to-Server Return**, there will be only **GET **request with query parameters, and it may fail depend on client network or device.\
\
For **Server-to-Server Callback**, you may decide the form of payment response depends on your settings at Riipay Merchant Portal. Generally, it can be either **GET** request with query parameters, **POST** **x-www-form-urlencoded** request or **POST** **JSON** request.

{% hint style="info" %}
Please take note, only completed transaction with "success" or "failed" status from gateway will have "callback" event. for case when there is no attempt to make payment, e.g. "Customer Cancel", or "Invalid Signature", there will be no **Server-to-Server Callback**, just **Client-to-Server Return**
{% endhint %}

### Response Parameters

| Fields                 | Type   | Description                                                                                                                     |
| ---------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| merchant\_code         | string | Your merchant code                                                                                                              |
| reference              | string | Your order unique reference number. Can be order ID                                                                             |
| description            | string | You order description                                                                                                           |
| currency\_code         | string | Your order currency code e.g. **MYR**                                                                                           |
| amount                 | string | <p>Total order amount. Format:</p><p>2 decimals without thousand separator.</p><p>eg: 12.00 or 12345.60</p><p>Minimum 1.00.</p> |
| transaction\_reference | string | Riipay transaction reference                                                                                                    |
| transaction\_datetime  | string | Riipay transaction datetime e.g. **2020-11-20 15:32:12**                                                                        |
| status\_code           | string | <p>Transaction status code.</p><p>Possible values: <code>F</code> , <code>A</code> , <code>S</code> . See Reference below.</p>  |
| status\_message        | string | Transaction status description.                                                                                                 |
| error\_code            | string | <p>Transaction error code. Empty if not applicable. See</p><p>Reference below.</p>                                              |
| error\_message         | string | Transaction error message. Empty if not applicable.                                                                             |
| signature              | string | Electronic signature. [See Response Signature.](payment-request.md#response-signature)                                          |

### Response Status Code

| Code | Description | Note                                                |
| ---- | ----------- | --------------------------------------------------- |
| F    | Failed      | Transaction is failed                               |
| A    | Accepted    | Transaction has been received and is under process. |
| S    | Success     | Transaction is successful.                          |

### Response Error Code

| Code | Description               |
| ---- | ------------------------- |
| 400  | Invalid Request Parameter |
| 401  | Invalid Signature         |
| 402  | Merchant Limit Exceeded   |
| 403  | User Limit Exceeded       |
| 404  | Invalid Merchant Code     |
| 405  | Payment not Allowed       |
| 406  | Payment Expired           |
| 409  | Invalid Session           |
| 410  | Cancelled by Customer     |
| 412  | Merchant Invalid Callback |
| 422  | Payment Failed            |
| 500  | Server Error              |
| 501  | Failed to Initialize      |
| 502  | Gateway Request Error     |
| 503  | Gateway Response Error    |
| 504  | Gateway Signature Error   |

## Response Signature

In order to ensure the data integrity of information passing from Riipay to you, we use electronic signature for validation and verification. The hash algorithm used is `md5`.

The signature value is a concatenated string consists of the following fields, in the following order:

* Merchant Code (provided by Riipay)
* Secret Key (provided by Riipay)
* reference
* currency\_code
* amount
* transaction\_reference
* status\_code

{% hint style="info" %}
amount have to be a string of 2 decimals without thousand separator. e.g.: 1234.00
{% endhint %}

It is a good practice to always make sure the signature you generate matches the signature retrieved from Riipay response before you proceed with the response data.

```php
public function isResponseValid($response)
{
    $reference = $response['reference'];
    $currency_code = $response['currency_code'];
    
    //convert amount to a string of 2 decimals with no thousand separator
    $amount = number_format($response['amount'], 2, '.', '');
    $transaction_reference = $response['transaction_reference'];
    $status_code = $response['status_code'];
    
    $hash = $merchant_code . $secret_key . $reference . $currency_code . $amount . $transaction_reference . $status_code;
    $signature = md5($hash);
    
    return $signature === $response['signature'];
}
```

## Sample Response

In this guide, we will demonstrate payment response in POST JSON format.

```javascript
{
    "merchant_code": "TEST",
    "reference": "SO20201109-01",
    "description": "Payment for order SO20201109-01",
    "currency_code": "MYR",
    "amount": 1234,
    "transaction_reference": "RP-20201109-ABCDEFGH",
    "transaction_datetime": "",
    "status_code": "F",
    "status_message": "Failed",
    "error_code": "405",
    "error_message": "User Limit Exceeded",
    "signature": "fe3c5fb7596fec4afeadd05abe4316ff"
}
```

Assume secret key is the same as our payment request sample a1b2c3d4e5. Response signature can be generated as follows:

```javascript
//retrieve raw data from JSON POST request
$json = file_get_contents('php://input');
//convert into PHP object
$response = json_decode($json);
$reference = $response['reference'];
$currency_code = $response['currency_code'];
//convert amount to a string of 2 decimals with no thousand separator
$amount = number_format($response['amount'], 2, '.', '');
$transaction_reference = $response['transaction_reference'];
$status_code = $response['status_code'];
$hash = $merchant_code . $secret_key . $reference . $currency_code . $amount . $transaction_reference . $status_code;
$signature = md5($hash);
```

{% hint style="success" %}
**For demonstration, the signature output would be**

fe3c5fb7596fec4afeadd05abe4316ff
{% endhint %}

Since the signature we generate is the same as the one in the response data, we can ensure that the response is valid. You may proceed with your own transaction process now.
