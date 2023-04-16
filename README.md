
# HimaPay Online Checkout
#### Last Updated 17th April, 2023

Our checkout API allows customers to make payments to merchants via the Himapay platform. The API is easy to use and is built for collections, checkout and billing.

## Endpoint

The endpoint for the checkout API is:

```bash
  https://app.himapay.co.ke/merchant/{merchant_id}/

```
Replace {merchant_id} with your merchant_id like this

```bash
  https://app.himapay.co.ke/merchant/3911ad7b-690e-4b13-92a1-fc9003be1cf7/
```


## Parameters
The following parameters can be passed as GET parameters in the URL:

```bash
amount: The amount to be paid in Kenyan shillings (KES).
description: A short description of the payment (e.g. "Payment for goods").
reference: An optional reference string that can be used to track the payment (e.g. an order number).
redirect: A redirect page, to receive Payment Confirmation Notification for that order.
```



## Usage/Examples

```bash
https://app.himapay.co.ke/merchant/3911ad7b-690e-4b13-92a1-fc9003be1cf7
?amount=10&description=Payment+for+goods&reference=123456789&redirect=https://yoururl.com/listener_endpoint

```


## API Reference

#### Here is a breakdown of the parameters



| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `merchant_id` | `string` | **Required**. Your Merchant ID. Will be assigned by Support |
| `amount` | `integer` | **Required**. The Amount to collect |
| `description` | `string` | **Required**. The description of the item being paid for. As will be seen by customers |
| `reference` | `string` | *Optional*. Your custom transaction identifier. Will be sent to your system, after transaction success |
| `redirect`  |  `url`   |  *Required if no URL is registered on merchants portal* |

## Callback IPN

Once a transaction is completed, we will send a notification to your system containing the following payload as a POST request.


```json
  {
      "amount":30,
      "reference":"TYUSJ8997YU",//The reference you sent in checkout request
      "externalRef":"RSWYUYIUHIUSK",//External Reference from MPESA,TKASH, AIRTEL MONEY
      "status":"success",//failed, pending
      "statusCode":"000000",//is success. 000001=pending, any other is failed
      "message":"Payment recieved successfully",//Or error message
      "signature":"rtyYIU6827DFghght239kRfwPo929323",//  a hmac sha256 hash comprising of externalRef + amount + reference + status, in that order, signed by your secret key
  }
```
For you to receive the callback, you need to register your callback endoint with us.
Write an email to pay@himapay.co.ke titled "Register Callback URL for {merchant_id}" 
### 
**Include* the endoint url, username and password for basic authentication.


- Your callback endpoint URL (e.g. https://example.com/ipn-handler)
- Username and password for basic authentication (if required)

To process the IPN payload sent by Himapay, you can follow the example code (PHP):

```php
<?php

// Retrieve the IPN payload sent by Himapay
$payload = file_get_contents('php://input');

// Decode the JSON payload into an associative array
$data = json_decode($payload, true);
$amount = $data['amount'];
$reference = $data['reference'];
$externalRef = $data['externalRef'];
$statusCode = $data['statusCode'];
$status = $data['status'];

$message = $data['message'];

$secret_key="";//Your Secret Key 

$hash = hash_hmac('sha256', $externalRef . $amount . $reference . $status, $secret_key);
if ($hash != $signature) {
  //the signature is invalid
  exit(1);
 }

// Check if the transaction was successful
if ($data['status'] === 'success') {
  // Extract the transaction details
 

  // TODO: Add your own code here to update your system or database with the transaction details
  // For example, you could log the transaction in your database or update the status of an order

  // Send a response to confirm receipt of the IPN
  http_response_code(200);
  echo 'IPN processed successfully';
} else {
  // The transaction failed or is still pending, so no action is required
  // Send a response to confirm receipt of the IPN
  http_response_code(200);
  echo 'IPN received but transaction was not successful';
}

?>
```
## SDKs & Plugins
1. HimaPay Woo-Commerce Checkout Plugin. [Download](https://app.himapay.co.ke/public/assets/himapay-woocommerce.zip)

## Authors

- [@mugwanjira](https://www.github.com/maina401)
