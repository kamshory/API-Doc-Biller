# Introduction

## Protocol

AltoPay Biller API used the HTTP protocol version 1.1. The method used for the request is `POST` or `PUT`.

## Messge Format

The message format used is JavaScript Object Notation (JSON). JSON is sent through the request body. The request body length must be included in the header.

## Request Headers

HTTP headers allow the client and the server to pass additional information with the request or the response. An HTTP header consists of its case-insensitive name followed by a colon "`:`", then by its value (without line breaks). Leading white space before the value is ignored. Some of headers used to make Inquiry Request and Payment are as follows:

1. **X-Alto-Timestamp**
**`X-Alto-Timestamp`** is the time stamp of the transaction with ISO Format in UTC. Time stamp can be used to validate the transaction time. For example `2019-12-31T23:59:59.999Z`.

2. **X-Alto-Key**
**`X-Alto-Key`** is used to identity the client.

3. **X-Alto-Signature**
**`X-Alto-Signature`** is required for transaction such as Inquiry and Payment. Signature using _hmac_ and _sha256_ with request method, relative path, times tamp, hash of entity body, username and password as parameters.
**Example**
```php
function signature($method, $path, $body, $timestamp, $apikey, $password)
{
    $hash = hash('sha256', $body);
    $string_to_sign = strtoupper($method).": ".$path.": ".$apikey.": ".$hash.": ".$timestamp;
    return bin2hex(hash_hmac('sha256', $string_to_sign, $password, true));
}

$request_body = json_encode($request_data);
$method       = "POST";
$path         = "/biller/";
$timestamp    = gmdate("Y-m-d\\TH:i:s").".000Z";
$password     = "--YOUR SECRET KEY HERE--";
$api_key      = "--YOUR API KEY HERE--";
$signature    = signature($method, $path, $request_body, $timestamp, $api_key, $password);
$headers      = array(
    'Content-Type: application/json',
    'Authorization: Basic '.base64_encode('kamshory:password'),
    'X-Alto-Signature: '.$signature,
    'X-Alto-Key: '.$api_key,
    'X-Alto-Timestamp: '.$timestamp
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "https://api-sandbox.altopay.id/v2/bill-payment/process-biller");
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
curl_setopt($ch, CURLOPT_USERAGENT, 'Planet POS');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_POSTFIELDS, $request_body);
$response = curl_exec($ch);
$err = curl_error($ch);
curl_close( $ch );
```
**Parameter Description**

| Parameter | Description | Example |
| -- | -- | -- |
| method | Request Method (GET, PUT, POST). | POST |
| path | Relative path of URL | /biller/json |
| body | Original request body without any modification | {“command”:”inquiry”, “data”:{}} |
| timestamp | ISO Time stamp. | 2019-07-07T23:59:59.999Z |
| apikey | API Key. It will be different for each client. |   |
| password | Validation key. Client can change this password anytime. |   |

**Note:**
a. timestamp and apikey are sent on request header.
b. password is not sent on request header.

5. **Content-type**
**`Content-type`** is filled with `json/application`.

5. **Content-encoding**
**`Content-type`** is filled with `identity`.

4. **Content-length**
**`Content-length`** is the length of the request body in bytes. The client must calculate the length of the  request body before sending it to the server. Therefore, the merchant may not send a request with partial content.

6. **Host**
**`Host`** is server address that will be accessed by merchant. This address contains: domain/subdomain, domain/subdomain and port, IP address, IP address and port. For example ``api-sandbox.altopay.id``.

## Request Body

Every request body always contains properties:

1. **`command`  (String)**
It is a command that must be execute by the server. This command must be write in lowercase letters. If the command is written incorrectly, the server will send a response with response code “Invalid Transaction” with an error message “Invalid Command”.
2. **`product_code` (String)**
It is the product code that will be requested on the transaction. If product is not found, AltoPay Biller will reject the transaction.

2. **`product_type` (String)**
It is the the alternative of the `product_code`. AltoPay Biller will find the product by combinating the `product_type`, `cutomer_number`, and `amount`. It will simplify the message for prepaid and Postpaid Airtime credit payment. If product is not found, AltoPay Biller will reject the transaction.

4. **`data` (Object)**
The data (object) contains the request message detail. Every command has different data properties.

## URL
The URL for production is different from the URL for development. The URL for production will be informed after signing the collaboration. For development purposes, use the development URL provided. This URL might change.

# Topology

The topology of ALTOPay Biller:  
![](architecture_flow.png)

# Transaction Flow

The following is ALTOPay Biller Architecture:  
![](transaction_flow.png)

# Product and Service

The list of products and services attached separately from this document is due to the large and growing numbers in accordance with the development of billers. Each product has a different message format according to the type of bill and also the policy of each biller.

The following are product categories based on the transaction process.

| No | Code | Description |
| -- | -- | -- |
| 1 | 1004 | Postpaid Electricity |
| 2 | 1005 | Prepaid Electricity |
| 3 | 1003 | Nonusage Electricity |
| 4 | 1010 | Prepaid Airtime |
| 5 | 1008 | Postpaid Airtime |
| 6 | 1009 | Mobile Data |
| 7 | 1007 | Water |
| 8 | 1006 | General Bill |

# General Message Format

The message format on one product will be different from the message format on other products. However, in general, each product has a uniform message format. This chapter will explain the general message format for all products.

## Inquiry

Inquiry is an electronic transaction to find out billing information for certain customer ID of the biller. Some products has no inquiry and some products require inquiry. 

### Request

#### a. Header

**Accept**

The `Accept` request HTTP header advertises which content types, expressed as MIME types, the client is able to understand. Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the `Content-Type` response header.

**Accept-encoding**

`Accept-encoding` is encoding that can be accepted by client. Acceptable encoding, namely:

1. `identity` (without compression)
2. `gzip` (with GZIP compression)

**Content-type**

The `Content-type` entity header indicates the size of the entity-body, in bytes, sent to the recipient.

**Content-Encoding**

The `Content-Encoding` entity header is used to compress the media-type. When present, its value indicates which encodings were applied to the entity-body. It lets the client know how to decode in order to obtain the media-type referenced by the `Content-Type` header.

**Content-length**

The `Content-length` entity header indicates the size of the entity-body, in bytes, sent to recipient.

**Connection**

The `Connection` general header controls whether or not the network connection stays open after the current transaction finishes. If the value sent is `keep-alive`, the connection is persistent and not closed, allowing for subsequent requests to the same server to be done.

**User-agent**

The `User-Agent` request header contains a characteristic string that allows the network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent.  

**Host**

`Host` is the name of the server accessed by client. From this header, the AltoPay can find out how client is connected to AltoPay.

**X-Alto-Key**

The `X-Alto-Key` is used to identity the client.

See **Request Headers** section on Chapter 1.

**X-Alto-Timestamp**

The `X-Alto-Timestamp` is one of signature component of the request to guarantee data integrity.

See **Request Headers** section on Chapter 1.

**X-Alto-Signature**

The `X-Alto-Signature` is signature of the request to guarantee data integrity. The parameters of the `X-Alto-Signature` are:

1. metod
2. path
3. body
4. timestamp
5. apikey
6. password

See **Request Headers** section on Chapter 1.

#### b. Body

| No | Parameter | Type | Description | Note |
| -- | -- | -- | -- | -- |
| 1 | command | String | Transaction command. See **Transaction Command List** | Mandatory |
| 2 | product_code | Numeric, can begin with 0 | Product code | Mandatory |
| 3 | product_type | Numeric, can begin with 0, alternative of product code | Product type | Conditional |
| 4 | data | Object | Container of the transaction data | Mandatory |
| 5 | `data`.customer_id | Numeric, can begin with 0 | Customer ID | Mandatory |
| 6 | `data`.date_time | String | Transmission date and time in GMT (Format: yyyy-MM-d’T’HH:mm:ss.SSS’Z’) | Mandatory |
| 7 | `data`.reference_number | String (32) | Reference number | Mandatory |

Note: 
1. The data length above is the maximum allowed. Shorter will be better.
2. Product type can be used for alternative of product code for prepaid airtime top up.

### Response

#### a. Header

**Content-type**

The `Content-type` entity header indicates the size of the entity-body, in bytes, sent to the recipient.

**Content-encoding**

`Content-encoding` is content encoding that sent by AltoPay. Content encoding, namely:

1. `identity` (without compression)
2. `gzip` (with GZIP compression)

**Content-length**

The `Content-length` entity header indicates the size of the entity-body, in bytes, sent to recipient.

**Date**

The `Date` general HTTP header contains the date and time at which the message was originated.

#### b. Body

| No | Parameter | Type | Description | Note |
| -- | -- | -- | -- | -- |
| 1 | command | String | Transaction command | Mandatory |
| 2 | product_code | Numeric, can begin with 0 | Product code | Mandatory |
| 3 | response_code | Numeric, can begin with 0 | Product code | Mandatory |
| 4 | response_text | String | Description of the response transaction | Mandatory |
| 5 | data | Object | Container of the transaction data | Mandatory |
| 6 | `data`.customer_id | Numeric, can begin with 0 | Customer ID | Mandatory |
| 7 | `data`.customer_name | String (32) | Customer name | Mandatory |
| 8 | `data`.amount | Numeric | Bill amount | Mandatory |
| 9 | `data`.date_time | String | Transmission date and time in GMT (Format: yyyy-MM-d’T’HH:mm:ss.SSS’Z’) | Mandatory |
| 10 | `data`.reference_number | String (32) | Reference number | Mandatory |

Note: The data length above is the maximum allowed. Shorter will be better.

## Payment

Payment is an electronic transaction to pay the bills for certain customer ID of the biller.

### Request

#### a. Header

**Accept**

The `Accept` request HTTP header advertises which content types, expressed as MIME types, the client is able to understand. Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Type response header.

**Accept-encoding**

`Accept-encoding` is encoding that can be accepted by client. Acceptable encoding, namely:

1. `identity` (without compression)
2. `gzip` (with GZIP compression)

**Content-type**

The `Content-type` entity header indicates the size of the entity-body, in bytes, sent to the recipient.

**Content-Encoding**

The `Content-Encoding` entity header is used to compress the media-type. When present, its value indicates which encodings were applied to the entity-body. It lets the client know how to decode in order to obtain the media-type referenced by the `Content-Type` header.

**Content-length**

The `Content-length` entity header indicates the size of the entity-body, in bytes, sent to recipient.

**Connection**

The `Connection` general header controls whether or not the network connection stays open after the current transaction finishes. If the value sent is `keep-alive`, the connection is persistent and not closed, allowing for subsequent requests to the same server to be done.

**User-agent**

The **User-Agent** request header contains a characteristic string that allows the network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent.  

**Host**

`Host` is the name of the server accessed by client. From this header, the AltoPay can find out how client is connected to AltoPay.

**X-Alto-Key**

The `X-Alto-Key` is used to identity the client.

See **Request Headers** section on Chapter 1.

**X-Alto-Timestamp**

The `X-Alto-Timestamp` is one of signature component of the request to guarantee data integrity.

See **Request Headers** section on Chapter 1.

**X-Alto-Signature**

The `X-Alto-Signature` is signature of the request to guarantee data integrity. The parameters of the X-Alto-Signature are:

1. metod
2. path
3. body
4. timestamp
5. apikey
6. password

See **Request Headers** section on Chapter 1.

#### Body

| No | Parameter | Type | Description | Note |
| -- | -- | -- | -- | -- |
| 1 | command | String | Transaction command | Mandatory |
| 2 | product_code | Numeric, can begin with 0 | Product code | Mandatory |
| 3 | product_type | Numeric, can begin with 0, alternative of product code | Product type | Conditional |
| 4 | data | Object | Container of the transaction data | Mandatory |
| 5 | `data`.customer_id | Numeric, can begin with 0 | Customer ID | Mandatory |
| 6 | `data`.customer_name | String | Customer ID | Mandatory for several billers |
| 7 | `data`.amount | Numeric | Payment amount | For several product, amount must be same with amount on inquiry response |
| 8 | `data`.date_time | String | Transmission date and time in GMT (Format: yyyy-MM-d’T’HH:mm:ss.SSS’Z’) | Mandatory |
| 9 | `data`.fwd_reference_number | String (32) | Reference number | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 8 | `data`.fwd_stan | Numeric | Payment amount | For several product, fwd_stan must be same with fwd_stan on inquiry response |

### Response

#### a. Header

**Content-type**

The `Content-type` entity header indicates the size of the entity-body, in bytes, sent to the recipient.

**Content-encoding**

`Content-encoding` is content encoding that sent by AltoPay. Content encoding, namely:

1. `identity` (without compression)
2. `gzip` (with GZIP compression)

**Content-length**

The `Content-length` entity header indicates the size of the entity-body, in bytes, sent to recipient.

**Date**

The `Date` general HTTP header contains the date and time at which the message was originated.

#### b. Body

| No | Parameter | Type | Description | Note |
| -- | -- | -- | -- | -- |
| 1 | command | String | Transaction command | Mandatory |
| 2 | product_code | Numeric, can begin with 0 | Product code | Mandatory |
| 3 | product_type | Numeric, can begin with 0, alternative of product code | Product type | Conditional |
| 4 | data | Object | Container of the transaction data | Mandatory |
| 5 | `data`.customer_id | Numeric, can begin with 0 | Customer ID | Mandatory |
| 6 | `data`.customer_name | String | Customer ID | Mandatory for several billers |
| 7 | `data`.amount | Numeric | Payment amount | For several product, amount must be same with amount on inquiry response |
| 8 | `data`.date_time | String | Transmission date and time in GMT (Format: yyyy-MM-d’T’HH:mm:ss.SSS’Z’) | Mandatory |
| 9 | `data`.reference_number | String (32) | Reference number | Mandatory |
| 10 | `data`.fwd_reference_number | String (32) | Reference number | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 11 | `data`.fwd_stan | Numeric | Payment amount | For several product, fwd_stan must be same with fwd_stan on inquiry response |

Note: The data length above is the maximum allowed. Shorter will be better.