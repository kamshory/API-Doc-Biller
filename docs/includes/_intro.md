# Introduction

## Protocol
ALTOPay Virtual Account API used the HTTP protocol version 1.1. The method used for the request is POST or PUT. There are two URL that will be used, URL for token and URL for Virtual Account.

## Message Format
ALTOPay Virtual Account used JavaScript Object Notation (JSON) that sent through the request body. The request body length must be included in the header.

## Request Headers
HTTP headers allow the client and the server to pass additional information with the request or the response. A HTTP header consists of its case-insensitive name followed by a colon ‘:’, then by its value (without line breaks), leading by space before the value is ignored. Some of headers used to make Inquiry Request and Payment are as follows:

### 1. X-Alto-Timestamp
X-Alto-Timestamp is the time stamp of the transaction with ISO format in UTC. Time stamp can be used to validate the transaction time.  
Example: 2019-12-31T23:59:59:999Z

### 2.	X-Alto-Key
API key of the merchant provided by ALTOPay. The merchant needs to verify that the API key is theirs. This timestamp is also required to generate signature sent in X-Alto-Signature header.

### 3.	X-Alto-Signature
>Example PHP:

```php
  function signature($method, $path, $body, $timestamp, $apikey, $password)
  {
    $hash = hash('sha256', $body);
    $string_to_sign = strtoupper($method)
      .":".$path
      .":".$apikey
      .":".$hash
      .":".$timestamp;
    return bin2hex(
      hash_hmac('sha256', $string_to_sign, $password, true)
      );
  }

  $password = "--YOUR SECRET KEY HERE--";
  $request_body = file_get_contents("php://input");
  $method = $_SERVER['REQUEST_METHOD'];
  $path = $_SERVER['REQUEST_URI'];
  $timestamp = $_SERVER['HTTP_X_TIMESTAMP'];
  $api_key = $_SERVER['HTTP_X_API_KEY'];
  $signature1 = $_SERVER['HTTP_X_SIGNATURE'];
  $signature2 = signature($method, $path, $request_body, $timestamp, $api_key, $password);

  if($signature1 == $signature2)
  {
    // TODO Your coude to process
  } 
  else
  {
    // TODO Your code to reject
  }
```

The signature is required for transaction such as Inquiry and Payment. Signature using hmac and sha256 with request method, relative path, time stamp, hash of entity body, username and password as parameters.

Parameter | Description | Example
----------|-------------|--------
method | Request method (GET, PUT, POST). | POST
path | Relative path of URL. | /virtual-account/
body | Original request body without any modification. | {“command”;inquiry”,”data”:{}}
timestamp | ISO timestamp. | 2019-07-07T23:59:59.999Z
apikey | API key. | 	
password | Client password. | 

**Note**:  
1. Timestamp and apikey sent on request header.  
2. If signature is invalid, merchant **MUST** reply with response **Unathorized**.  


### 4.	Content-length
Content length is the length of the request body in bytes. Client must calculate the length of the request body before send to the server. Therefore, ALTOPay will not send a request with partial content.
### 5.	Content-type
Content type filled with json/application.
### 6.	Host 
Server address that will be accessed by ALTOPay Virtual Account. This address contains: domain/subdomain, domain/subdomain and port, IP address, IP address and port.


##	Request Body
Every request body always contains these two properties:

### 1.	Command (string)
It is command that must be execute by the server. This command must be written in lowercase letters. If the command is written incorrectly, the server will send a response with response code “Invalid Transaction”, with an error message “Invalid Command”.

### 2.	Data (object)
The data (object) contains details of the request message. Every command has different data properties.

##	Languages
ALTOPay Virtual Account supports multilingual. Some banks use more than one language in one message while several other banks choose one language according to their customer’s preferred language when making transactions.
ALTOPay requires merchants to fill a minimum 2 languages (Bahasa Indonesia and English) in each response

##	URL
Merchants must provide at least an URL to accept request for inquiry and payment. Merchants can unify or separate the URL. If merchants use one URL for inquiry and payment, merchants can differentiate between the two requests from the command parameter on the body request received.

##	Bill
Bill can be Closed Amount, Open Amount, and Mix Amount. The following are the bill amount matrix for each type:

No | Bill Type | Amount | Min Amount | Max Amount | Description
---|-----------|--------|------------|------------|------------
1 | Closed Amount | > 0 | 0 or not sent | 0 or not send | Bills appear, customer confirms payment
2 | Open Amount | 0 | > 0 | > 0 | No bills and Customers can enter the amount
3 | Mix Amount | 0 | > 0 | > 0 | Bills appear but the customer can input the amount

**Note**:  
1. If amount of the bill is filled, bill is classified as Closed Amount.  
2. If amount of the bill is filled by 0, bill is classified as Open Amount.  
3. For Open Amount, maximum and minimum amount must be filled.  

ALTOPay Virtual Account provides 3 codes for 3 types of bill.  
1. 70070AA for closed amount  
2. 70072AA for open amount  
3. 70071AA for mix amount  

Where AA is payment gateway code.  
Merchant must generate virtual account number with prefix above. The prefix number will be informed later.  
Currently ALTOPay Virtual Account only supports single bill payments because our partner banks do not yet support multiple bill payments. For this reason, merchants can only use a single bill for one virtual account number.  


##	Virtual Account Number
AltoPay Virtual Account Numbering is as follows:  
![](va_numbering.png)

**Note**:  
1. X = Company Code Alto at the bank  
2. Y = Payment Gateway Code on Alto  
3. 1 - 8 = Customer Code at Merchant  

**Example**:  
**12345** 12 **87654321**

12345 is the Company Code  
12 is the Payment Gateway Code  
87654321 is the Customer Code  

VA Number (Virtual Account Number) is used to make inquiries and payments to banks. The bank will recognize the company code and then forward the request to the company (for example AltoPay). AltoPay will forward the request to the Payment Gateway (for example, with code 12). Payment Gateway with code 12 will continue to read the billing data in the database with customer number 87654321.