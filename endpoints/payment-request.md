# Payment Request

{% api-method method="get" host="https://{{BASE\_URL}}" path="/v1/payment" %}
{% api-method-summary %}
Initiate a payment request
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to initial a new payment request.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="merchant\_code" type="string" required=true %}
Your merchant code
{% endapi-method-parameter %}

{% api-method-parameter name="reference" type="string" required=true %}
Your order unique reference number. Can be order ID
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
Your order description
{% endapi-method-parameter %}

{% api-method-parameter name="currency\_code" type="string" required=true %}
Your order currency code e.g. **MYR**
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="number" required=true %}
Total order amount. Format:  
2 decimals without thousand separator.  
eg: 12.00 or 12345.60  
Minimum 1.00.
{% endapi-method-parameter %}

{% api-method-parameter name="signature" type="string" required=true %}
Electronic signature. see Request Signature
{% endapi-method-parameter %}

{% api-method-parameter name="customer\_name" type="string" required=false %}
Customer name
{% endapi-method-parameter %}

{% api-method-parameter name="customer\_email" type="string" required=false %}
Customer email address
{% endapi-method-parameter %}

{% api-method-parameter name="customer\_phone" type="string" required=false %}
Customer phone number
{% endapi-method-parameter %}

{% api-method-parameter name="customer\_ip" type="string" required=false %}
Customer IP Address
{% endapi-method-parameter %}

{% api-method-parameter name="return\_url" type="string" required=false %}
Return URL. Submit this field if you wish to override your default return URL in dashboard profile settings.
{% endapi-method-parameter %}

{% api-method-parameter name="callback\_url" type="string" required=false %}
Callback URL. Submit this field if you wish to override your default callback URL in dashboard profile settings.
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

### Merchant Return or Callback

<table>
  <thead>
    <tr>
      <th style="text-align:left">Fields</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">merchant_code</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Your merchant code</td>
    </tr>
    <tr>
      <td style="text-align:left">reference</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Your order unique reference number. Can be order ID</td>
    </tr>
    <tr>
      <td style="text-align:left">description</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">You order description</td>
    </tr>
    <tr>
      <td style="text-align:left">currency_code</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Your order currency code e.g. <b>MYR</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">amount</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Total order amount. Format:</p>
        <p>2 decimals without thousand separator.</p>
        <p>eg: 12.00 or 12345.60</p>
        <p>Minimum 1.00.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">transaction_reference</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Riipay transaction reference</td>
    </tr>
    <tr>
      <td style="text-align:left">transaction_datetime</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Riipay transaction datetime e.g. <b>2020-11-20 15:32:12</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">status_code</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>A - Approved</p>
        <p>S - Success</p>
        <p>F - Failed</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">status_message</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Status description</td>
    </tr>
    <tr>
      <td style="text-align:left">error_code</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Error Code</td>
    </tr>
    <tr>
      <td style="text-align:left">error_message</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Error Message</td>
    </tr>
    <tr>
      <td style="text-align:left">signature</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Response signature</td>
    </tr>
  </tbody>
</table>



