# Getting Started

Riipay provides staging and production environments.

* Staging environment - used for testing and experimenting with the integration. There are no real transactions happening on it.
* Production environment - real environment.

## Setup Riipay Account

Merchant dashboard login credentials will be provided after successful registration.

Based on the environment you are using, you can visit the dashboard through the links:

* **Staging dashboard** [`https://merchant.uat.riipay.my`](https://merchant.uat.riipay.my)
* **Production dashboard** [`https://merchant.riipay.my`](https://merchant.riipay.my)

## Configure Merchant Portal

In the dashboard profile settings, you will need to specify:

* **Return URL** - a page on your website to redirect your customer after they complete or cancel the payment.
* **Callback URL** - an endpoint to receive or updated payment information from Riipay after transactions.
* **Callback Method** - the form of callback requests you wish to receive. It can be **GET** request, **POST x-www-form-urlencoded** request, or **POST JSON** request. 
* **Bank Account Details** - for settlement purpose.

{% hint style="info" %}
The default Callback Method is **POST JSON** request.
{% endhint %}

## Prerequisites

Prior to sending payment request, you are required to have the following information ready:

* Merchant Code 
* Secret Key

You may obtain these information from your dashboard profile page.

## Service Endpoint

Throughout this guide, we will use `{{BASE_URL}}` as a placeholder. You should use the domains that match the environment you are using.

* Staging URL `https://secure.uat.riipay.my`
* Production URL `https://secure.riipay.my`



