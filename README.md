
# HimaPay Online Checkout


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
```



## Usage/Examples

```bash
https://app.himapay.co.ke/merchant/3911ad7b-690e-4b13-92a1-fc9003be1cf7
?amount=10&description=Payment+for+goods&reference=123456789

```


## API Reference

#### Here is a breakdown of the parameters



| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `merchant_id` | `string` | **Required**. Your Merchant ID. Will be assigned by Support |
| `amount` | `integer` | **Required**. The Amount to collect |
| `description` | `string` | **Required**. The description of the item being paid for. As will be seen by customers |
| `reference` | `string` | *Optional*. Your custom transaction identifier. Will be sent to your system, after transaction success |

## Callback IPN
Once a transaction is completed, we will send a notification to your system containing the following payload.

```json
  {
      "amount":30,
      "reference":"TYUSJ8997YU",//The reference you sent in checkout request
      "externalRef":"RSWYUYIUHIUSK",//External Reference from MPESA,TKASH, AIRTEL MONEY
      "status":"success",//failed, pending
      "statusCode":"000000"//is success. 000001=pending, any other is failed
      "message":"Payment recieved successfully"//Or error message
  }
```
For you to receive the callback, you need to register your callback endoint wit us.
Write an email to pay@himapay.co.ke titled "Register Callback URL for {merchant_id}" 
### 
**Include* the endoint url, username and password for basic authentication.

## Authors

- [@mugwanjira](https://www.github.com/maina401)

