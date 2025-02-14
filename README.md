![Zarinplus Logo](https://www.zarinplus.com/wp-content/uploads/2021/10/ZarinPlus-logo-light.svg?nocache)

# Zarinplus Docs

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./license)

## Table of Contents

* [Payment](#payment)
	* [Create Payment Request Via API](#create-payment-request-via-api)
	* [Verify Transaction Via API](#verify-transaction-via-api)
	* [Reverse Transaction Via API](#reverse-transaction-via-api)

## Payment

This part of document is for merchants _(Who accept payments via points)_, If you would like to create payment requests from your server and verify the transactions, Please read this document to understand how to implement payment via request endpoint.

### Create Payment Request Via API
Get your `{private token}` from your merchant account and replace it in the request endpoint. 

#### Request Endpoint

	https://api.zarinplus.com/{private-token}/payment/request.json
	

#### Body

Send these parameters to the request endpoint via `GET` or `POST` method.

	{
		"currency" : "IRR",
		"amount" : 50000,
		"cancel" : "http://www.mystore.com/cancel/",
		"success" : "http://www.mystore.com/success/",
		"item" : "5000 Toman Voucher",
		"cellphone" : 989121111111,
		"email" : "yourname@domain.com"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"currency" : {
			"type" : "string",
			"description" : "Currency is required, Those currency codes are supported IRR, IRT, POT, USD"
		},
		"amount" : {
			"type" : "float"
			"description" : "Amount is required"
		},
		"cancel" : {
			"type" : "string"
			"description" : "The cancel URL"
		},
		"success" : {
			"type" : "string"
			"description" : "The success URL"
		},
		"item" : {
			"type" : "string"
			"description" : "Details of transaction"
		},
		"cellphone" : {
			"type" : "integer"
			"description" : "buyer cellphone"
		},
		"email" : {
			"type" : "string"
			"description" : "buyer email address"
		}
	}
  
#### Good Response

If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/zarinplus/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Get the authority code",
		"authority" : "f72705fe8a9bfc8895cc5dac121931f696d00b61"
	}

#### Redirect To Offerwall

Now you should build the offerwall's URL by replacing `{authority}` with the authority you received from previous step and then redirect to offerwall.

Also you must store the authority value for this transaction in your database for next action.

	https://api.zarinplus.com/?authority={authority}


### Verify Transaction Via API

After the buyers make the payments with their points, We redirect them to the `success` URL you previously provided to us.
Now you should verfiy the payment transaction.

	http://www.mystore.com/success/?authority={authority}

#### Verify Endpoint

	https://api.emtiyaz.app/{private token}/payment/verify.json

#### Body

Now please, Send `authority` back for verfiy.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"authority" : "f72715fe8a1bfc8895cc5dac121931f696d00b61"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"authority" : {
			"type" : "string"
			"description" : "Authority that you received on your success URL"
		}
	}

#### Good Response

Well now, The payment is complete and verified, It's time to deliver your goods or services to buyer.
If you send correct request then the response should be same thing like that, Please watch [Sample Codes](https://github.com/emtiyaz-app/samplecodes/tree/master/php/payment).

	{
		"status" : "200",
		"message" : "Successful",
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c"
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "name@email.com",
		"cellphone" : "989121111111"
	}

### Reverse Transaction Via API

You can reverse any successful transaction. 

#### Reverse Endpoint

	https://api.emtiyaz.app/{private token}/payment/reverse.json

#### Body

Now please, Send `authority` OR `reference` back for reverse.
Send this parameters to the verify endpoint via `GET` or `POST` method.

	{
		"reference" : "46a1e78525619a25b368c32b9ba11b92f6063e0c"
	}

#### Body Schema

This schema define the each parameter's type and value.

	{
		"reference" : {
			"type" : "string"
			"description" : "The reference that you received on verify method"
		}
	}

#### Good Response

Well now, The transaction successfully reversed.

	{
		"status" : "500",
		"message" : "Reversed",
		"amount" : "50000",
		"currency" : "IRR",
		"item" : "5000 Toman Voucher",
		"email" : "name@email.com",
		"cellphone" : "989121111111"
	}


## List of Response Codes

|  Code	    | Message       |
| ------------- |:-------------:|
| 401 | Invalid point value |
| 402 | Invalid amount or currency |
| 403 | Invalid token |
| 200 | Successful |
| 471 | Unknown error |
| 481 | Invalid authority |
| 482 | Invalid authority |
| 300 | Used authority |



