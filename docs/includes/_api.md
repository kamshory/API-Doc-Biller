# API Format

## Inquiry

Inquiry is an electronic transaction to find out billing information for certain virtual account numbers. 

Setting | Value
--------|------
HTTP Method | POST
Path | /plugins/va/inquiry/index.php
Host-Sandbox | api-sandbox.altopay.id

### Inquiry Request Message

>Example Request Message

```http
POST /plugins/va/inquiry/index.php HTTP/1.1
X-Alto-Signature: 3f93fd471e40130fa2db94b387d6590b92cd756cc5e7f955abbea41a8879e4fc
Accept-encoding: identity
Content-type: application/json; charset="utf-8"
Accept-language: en-US, id-ID
X-Alto-Key: Planet
Accept: application/json
Connection: close
User-agent: ALTOPay Virtual Account version 1.1
X-Alto-Timestamp: 2019-07-12T04:23:05.571Z
Content-Length: 375
Host: store.planetedu.id
```
```json
{
    "command": "inquiry",
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "pg_code": "12",
        "customer_number": "71199991",
        "request_id": "20190712112305441048010",
        "reference_number": "000000000564",
        "channel_type": "6014",
        "date_time": "2019-07-12T11:23:05.000Z"
    }
}
```

**Header**:

Parameter | Description | Data Type | Type
----------|-------------|-----------|-----
X-Alto-Signature | Signature of the request to guarantee the data integrity. The following are parameters of the X-Alto-Signature: <br> 1. Method <br> 2.	Path <br> 3.	Body <br> 4.	Timestamp <br> 5.	Apikey <br> 6.	Password | String | M
Accept-encoding	| Encoding that can be accepted by ALTOPay. Acceptable encoding: <br> 1.	identity (without compression) <br> 2.	gzip (with GZIP compression) | String | M
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Accept-language | The Accept-Language request HTTP header advertises which languages the client is able to understand, and which locale variant is preferred. (By languages, we mean natural languages, such as English, and not programming languages.) Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according to their user interface language and even if a user can change it, this happens rarely (and is frowned upon as it leads to fingerprinting). <br> This header is a hint to be used when the server has no way of determining the language via another way, like a specific URL, that is controlled by an explicit user decision. It is recommended that the server never overrides an explicit decision. The content of the Accept-Language is often out of the control of the user (like when traveling and using an Internet Cafe in a different country); the user may also want to visit a page in another language than the locale of their user interface. <br> If the server cannot serve any matching language, it can theoretically send back a 406 (Not Acceptable) error code. But, for a better user experience, this is rarely done and more common way is to ignore the Accept-Language header in this case. | String | O
X-Alto-Key | API key of the merchant provided by ALTOPay. The merchant needs to verify that the API key is theirs. This timestamp is also required to generate signature sent in X-Alto-Signature header. | String | M
Accept | The accept request HTTP header advertises which content types, expressed as MIME types, the client is able to understand. Using content negotiation, the server then selects one of the proposals, use it and informs the client of its choice with the Content-type response header. | String | M
Connection | The connection general header controls whether or not the network connection stays open after the current transaction finishes. If the value sent is keep-alive, the connection is persistent and not closed. Allowing for subsequent request to the same server to be done. | String | M
User-agent | The User-agent request header contains a characteristics string that allows the network protocol peers to identify the application type, operating system, software vendor of software version of the requesting software user agent. ALTOPay will send specific User-agent to merchant. | String | M
X-Alto-Timestamp | Timestamp in GMT sent by ALTOPay. This timestamp is required to generate signature. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSSZ | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M
Host | Host is the name of the server accessed by ALTOPay. From this header, the merchant server can find out how ALTOPay is connected to the merchant server. | String | M

**Body**:

Parameter | Description | Data Type | Type
----------|-------------|-----------|-----
command | Transaction command | String | M
data | Container of the transaction data | Object | M
&nbsp;&nbsp;bank_code | Bank code from where ALTOPay get the inquiry request. Can begin with 0. | Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number sent by bank | String(20) | M
&nbsp;&nbsp;pg_code | Payment gateway code on ALTOPay virtual account. Can begin with 0. Each payment gateway will have a unique payment gateway code. The payment gateway just needs to verify that the payment gateway is theirs. | Numeric(2) | M
&nbsp;&nbsp;customer_number | Customer number. Can begin with 0 | Numeric(12) | M
&nbsp;&nbsp;request_id | Request ID sent by ALTOPay. If the bank send3 request ID, ALTOPay will forward this request ID. If the bank does not send request ID, ALTOPay will generate a request ID. | String(32) | O
&nbsp;&nbsp;reference_number | Reference number of the inquiry request. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the inquiry. | Numeric(4) | O
&nbsp;&nbsp;date_time | Transmission date and time in GMT. Format: yyyy-mm-dd’T’hh:mm:ss.SSSZ | String(24) | M


### Inquiry Response Message

> Example Response Message

```http
HTTP/1.1 200 OK
X-Powered-By: PHP/5.6.35
Content-Type: application/json
Content-Length: 2662
```
```json
{
    "command": "inquiry",
    "response_code": "00",
    "response_text": "Success",
    "message": {
        "id": "Sukses",
        "en": "Success"
    },
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "merchant_code": "123",
        "customer_number": "71199991",
        "customer_name": "Mas Roy",
        "customer_phone": "0808080808",
        "customer_email": "test@example.com",
        "request_id": "20190712112305441048010",
        "reference_number": "000000000564",
        "channel_type": "6014",
        "time_stamp": "2019-07-12T04:23:05.000Z",
        "total_amount": 151975000,
        "currency_code": "IDR",
        "bill_list": [
            {
                "bill_number": "0001",
                "bill_amount": 1500000,
                "bill_max_amount": 0,
                "bill_min_amount": 0,
                "bill_description": {
                    "id": "Rock Pi 4",
                    "en": "Rock Pi 4"
                }
            }
        ]
    }
}
```

**Header**:  

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8.	| String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command	| Transaction command. | String | M
response_code	| Inquiry response code indicate the transaction is success or not.	| String(2)	| M
response_text	| Description of response code. | String(20)| M
message | Message to be displayed to the customer. | Multilingual object | M
data | Container of the transaction data. | Object | M
&nbsp;&nbsp;bank_code	| Bank code from where ALTOPay get the inquiry request. <br> Can begin with 0. | Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number sent by bank. | String(20) | M
&nbsp;&nbsp;pg_code |	Payment gateway code on ALTOPay virtual account. <br> Can begin with 0. Each payment gateway will have a unique payment gateway code. The payment gateway just needs to verify that the payment gateway is theirs. | Numeric(2) | M
&nbsp;&nbsp;customer_number	| Customer number. <br> Can begin with 0 | Numeric(12) | M
&nbsp;&nbsp;customer_name | Customer name. | String(50) | M
&nbsp;&nbsp;customer_phone | Customer phone number. <br> Can begin with 0. | Numeric(13) | O
&nbsp;&nbsp;customer_email | Customer email address. | String(80) | O
&nbsp;&nbsp;request_id | Request ID sent by ALTOPay for current transaction. | String(32) | O
&nbsp;&nbsp;reference_number | Transaction reference number sent by ALTOPay for current transaction. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the inquiry. | Numeric(4) | O
&nbsp;&nbsp;time_stamp | Response timestamp in GMT. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSSZ | String(24) | M
&nbsp;&nbsp;total_amount | Total amount should pay by customer.
&nbsp;&nbsp;currency_code | Currency code. | Number(3) | M
&nbsp;&nbsp;bill_list | Bill list. | Object | M
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. Mandatory if bill exists. <br> Can begin with 0. | Numeric(12) | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_amount | Bill amount. | Numeric | O
&nbsp;&nbsp;&nbsp;&nbsp;bill_max_amount | Maximum bill amount. | Numeric | O
&nbsp;&nbsp;&nbsp;&nbsp;bill_min_amount | Minimum bill amount. | Numeric | O
&nbsp;&nbsp;&nbsp;&nbsp;bill_description | Bill description to be displayed to the customer. Mandatory if bill exists. | Multilingual object | C


## Payment
Payment is an electronic transaction to make bill payments. Bank will display the payment optional to the customer on accordance with the results of the inquiry to the merchant. Thus, inquiry is a mandatory process.

Setting | Value
--------|------
HTTP Method | POST
Path | /plugins/va/payment/index.php
Host-Sandbox | api-sandbox.altopay.id

### Payment Request Message

> Example Request Message

```http
POST /plugins/va/payment/index.php HTTP/1.1
X-Alto-Signature: c8951cbf58ace7da93862cb608b67a38f2ea555feb114807e345dd98acc325f8
Accept-encoding: identity
Content-type: application/json; charset="utf-8"
Accept-language: en-US, id-ID
X-Alto-Key: Planet
Accept: application/json
Connection: close
User-agent: ALTOPay Virtual Account version 1.1
X-Alto-Timestamp: 2019-07-12T04:23:14.769Z
Content-Length: 1154
Host: store.planetedu.id
```
```json
{
    "command": "payment",
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "pg_code": "12",
        "customer_number": "71199991",
        "customer_name": "Mas Roy",
        "request_id": "12345",
        "reference_number": "000000000565",
        "channel_type": "6014",
        "date_time": "2014-03-15T09:07:05.000Z",
        "total_amount": 1500000,
        "paid_amount": 1500000,
        "currency_code": "IDR",
        "bill_list": [
            {
                "bill_number": "0001",
                "bill_amount": 1500000,
                "payment_reference": "34264823420"
            }
        ]
    }
}
```

**Header**:  

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
X-Alto-Signature | Signature of the request to guarantee the data integrity. The following are parameters of the X-Alto-Signature: <br> 1.	Method <br> 2.	Path <br> 3.	Body <br> 4.	Timestamp <br> 5.	Apikey <br> 6.	Password	| String | M
Accept-encoding	| Encoding that can be accepted by ALTOPay. Acceptable encoding: <br> 1.	identity (without compression) <br> 2.	gzip (with GZIP compression) | String | M
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Accept-language | The Accept-Language request HTTP header advertises which languages the client can understand, and which locale variant is preferred. (By languages, we mean natural languages, such as English, and not programming languages.) Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according to their user interface language and even if a user can change it, this happens rarely (and is frowned upon as it leads to fingerprinting). <br> This header is a hint to be used when the server has no way of determining the language via another way, like a specific URL, that is controlled by an explicit user decision. It is recommended that the server never overrides an explicit decision. The content of the Accept-Language is often out of the control of the user (like when traveling and using an Internet Cafe in a different country); the user may also want to visit a page in another language than the locale of their user interface. <br> If the server cannot serve any matching language, it can theoretically send back a 406 (Not Acceptable) error code. But, for a better user experience, this is rarely done, and more common way is to ignore the Accept-Language header in this case. | String | O
X-Alto-Key | API key of the merchant provided by ALTOPay. The merchant needs to verify that the API key is theirs. This timestamp is also required to generate signature sent in X-Alto-Signature header. | String | M
Accept | The accept request HTTP header advertises which content types, expressed as MIME types, the client can understand. Using content negotiation, the server then selects one of the proposals, use it and informs the client of its choice with the Content-type response header. | String | M
Connection | The connection general header controls whether the network connection stays on after the current transaction finishes. If the value sent is keep-alive, the connection is persistent and not closed. Allowing for subsequent request to the same server to be done. | String | M
User-agent | The User-agent request header contains a characteristics string that allows the network protocol peers to identify the application type, operating system, software vendor of software version of the requesting software user agent. ALTOPay will send specific User-agent to merchant. | String | M
X-Alto-Timestamp | Timestamp in GMT sent by ALTOPay. This timestamp is required to generate signature. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSSZ | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M
Host | Host is the name of the server accessed by ALTOPay. From this header, the merchant server can find out how ALTOPay is connected to the merchant server. | String | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | Transaction command. | String | M
data | Container of the transaction data. | Object	
&nbsp;&nbsp;bank_code | Bank code from where ALTOPay get the payment request. <br> Can begin with 0. | Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number set by bank. | String(20) | M
&nbsp;&nbsp;pg_code | Payment gateway code on ALTOPay virtual account. <br> Can begin with 0. Each payment gateway will have a unique payment gateway code. The payment gateway &nbsp;&nbsp;just needs to verify that the payment gateway is theirs. | Numeric(2)| M
&nbsp;&nbsp;customer_number | Customer number. can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;customer_name | Customer name from the inquiry response. Some banks may be trimming the customer name due their system spec. Some banks also may not send the customer name. The merchant must check the customer name if exists. | String(50) | O
&nbsp;&nbsp;request_id| Request ID sent by ALTOPay. If the bank send request ID, ALTOPay will forward this request ID. If the bank does not send request ID, ALTOPay will generate a request ID. | String(32) | O
&nbsp;&nbsp;reference_number | Reference number of the payment request. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the payment. | Numeric(4) | O
&nbsp;&nbsp;date_time | Transmission date and time in GMT.  <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSS’Z’. | String(24) | M
&nbsp;&nbsp;total_amount | Total amount of all bills. It will be same with the total amount from the previous inquiry response. | Numeric | O
&nbsp;&nbsp;paid_amount | Total amount paid by customer for current transaction. | Numeric | M
&nbsp;&nbsp;currency_code | Currency code of the transaction. If the currency code of the payment sent by ALTOPay is different with the currency code of the bill, the merchant needs to convert the amount to determine that the amount sent by the customer is enough to pay the bill. | String(3) | M
&nbsp;&nbsp;bill_list | Bill list. | Object	
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. Can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;&nbsp;&nbsp;bill_amount | Amount paid by the customer for the bill. It can be different with the bill amount on the merchant. | Numeric | M
&nbsp;&nbsp;&nbsp;&nbsp;payment_reference | Payment reference from bank. Some banks do not send  the payment reference but ALTOPay will fill it with reference number if bank does not send payment reference. | String(32) | O

### Payment Response Message

>Example Response Message

```http
HTTP/1.1 200 OK
X-Powered-By: PHP/5.6.35
Content-Type: application/json
Content-Length: 2515
```
```json
{
    "command": "payment",
    "response_code": "00",
    "response_text": "Success",
    "message": {
        "id": "Sukses",
        "en": "Success"
    },
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "pg_code": "12",
        "customer_number": "71199991",
        "customer_name": "Mas Roy",
        "merchant_id":"123",
        "merchant_name":"Tes Merchant",
        "request_id": "12345",
        "reference_number": "000000000565",
        "channel_type": "6014",
        "time_stamp": "2019-07-12T04:23:14.000Z",
        "total_amount": 1500000,
        "paid_amount": 1500000,
        "currency_code": "IDR",
        "bill_list": [
            {
                "bill_number": "0001",
                "bill_amount": 1500000,
                "bill_description": {
                    "id": "Rock Pi 4",
                    "en": "Rock Pi 4"
                },
                "payment_status": "00",
                "payment_reference": "34264823420",
                "payment_message": {
                    "id": "Sukses",
                    "en": "Success"
                }
            }
        ]
    }
}
```

**Header**:

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | The transaction command that indicates the transaction. <br> Value: authentication-request-repeat. | String (30) | M
response_code | Payment response code indicates whether transaction is success or not. | String(3) | M
response_text | Response code description. | String(50) | M
message | Multilingual message to be displayed to the customer.	| Multilingual object | M
data | Container of the transaction data.	| Object	
&nbsp;&nbsp;bank_code	| Bank code from where customer request inquiry. <br> Can begin with 0.	| Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number sent by bank. | String(20) | M
&nbsp;&nbsp;pg_code | Payment gateway code on ALTOPay virtual account. <br> Can begin with 0.	| Numeric(2) | M
&nbsp;&nbsp;customer_number	| Customer number. can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;customer_name | Customer name	| Numeric(50)| 	M
&nbsp;&nbsp;request_id | Request ID sent by ALTOPay for current transaction. | String(32) | O
&nbsp;&nbsp;reference_number | Transaction reference number sent by ALTOPay for current transaction. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the inquiry. | Numeric(4) | O
&nbsp;&nbsp;time_stamp | Response date and time in GMT. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSS’Z’. | String(24) | M
&nbsp;&nbsp;total_amount | Total amount of all bills sent by ALTOPay for current transaction. | Numeric | O
&nbsp;&nbsp;paid_amount | Total amount paid customer sent by ALTOPay for current transaction. | Numeric | M
&nbsp;&nbsp;currency_code | Currency code sent by ALTOPay for current transaction. | String(3) | M
&nbsp;&nbsp;bill_list | Bill list. | Object	
&nbsp;&nbsp;&nbsp;&nbsp;bill_amount | Bill amount. Mandatory if bill exists. | Numeric | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. Mandatory if bill exists. <br> Can begin with 0. | Numeric(12) | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. Mandatory if bill exists. <br> Can begin with 0. | Numeric(12) | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_description | Bill description to be displayed to the customer. Mandatory if bill exists. | Multilingual object | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_status | Payment status. | String(3) | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_message | Multilingual message to be displayed to the customer. | Multilingual object | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_rereference | Payment reference sent by ALTOPay if exists. | String(20) | C


##	Check Status

The check status is used to ascertain whether the previous transaction was successful or failed. Check the status of sending parameters in the form of transaction reference numbers. Merchant must ensure the status of payment transactions that have been made. The Merchant must respond to this check status with the status of success or failure.
Check Status is performed if Alto does not receive a response from the merchant due to many possibilities such as problems in the application or on the network. 

Setting | Value
--------|------
HTTP Method | POST
Path | /plugins/va/payment/index.php
Host-Sandbox | api-sandbox.altopay.id


### Check Status Request Message

>Example Request Message

```http
POST /plugins/va/payment/index.php HTTP/1.1
X-Alto-Signature: c8951cbf58ace7da93862cb608b67a38f2ea555feb114807e345dd98acc325f8
Accept-encoding: identity
Content-type: application/json; charset="utf-8"
Accept-language: en-US, id-ID
X-Alto-Key: Planet
Accept: application/json
Connection: close
User-agent: ALTOPay Virtual Account version 1.1
X-Alto-Timestamp: 2019-07-12T04:23:14.769Z
Content-Length: 1154
Host: store.planetedu.id
```
```json
{
    "command": "check-status",
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "pg_code": "12",
        "customer_number": "71199991",
        "customer_name": "Mas Roy",
        "request_id": "12345",
        "reference_number": "000000000565",
        "channel_type": "6014",
        "date_time": "2014-03-15T09:07:05.000Z",
        "total_amount": 1500000,
        "paid_amount": 1500000,
        "currency_code": "IDR",
        "bill_list": [
            {
                "bill_number": "0001",
                "bill_amount": 1500000,
                "payment_reference": "34264823420"
            }
        ]
    }
}
```

**Header**:

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
X-Alto-Signature | Signature of the request to guarantee the data integrity. The following are parameters of the X-Alto-Signature: <br> 1.	Method <br> 2.	Path <br> 3.	Body <br> 4.	Timestamp <br> 5.	Apikey <br> 6.	Password | String | M
Accept-encoding | Encoding that can be accepted by ALTOPay. Acceptable encoding: <br> 1.	identity (without compression) <br> 2.	gzip (with GZIP compression) | String | M
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Accept-language | The Accept-Language request HTTP header advertises which languages the client can understand, and which locale variant is preferred. (By languages, we mean natural languages, such as English, and not programming languages.) Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according to their user interface language and even if a user can change it, this happens rarely (and is frowned upon as it leads to fingerprinting). <br> This header is a hint to be used when the server has no way of determining the language via another way, like a specific URL, that is controlled by an explicit user decision. It is recommended that the server never overrides an explicit decision. The content of the Accept-Language is often out of the control of the user (like when traveling and using an Internet Cafe in a different country); the user may also want to visit a page in another language than the locale of their user interface. <br> If the server cannot serve any matching language, it can theoretically send back a 406 (Not Acceptable) error code. But, for a better user experience, this is rarely done, and more common way is to ignore the Accept-Language header in this case. | String | O
X-Alto-Key | API key of the merchant provided by ALTOPay. The merchant needs to verify that the API key is theirs. This timestamp is also required to generate signature sent in X-Alto-Signature header. | String | M
Accept | The accept request HTTP header advertises which content types, expressed as MIME types, the client can understand. Using content negotiation, the server then selects one of the proposals, use it and informs the client of its choice with the Content-type response header. | String | M
Connection | The connection general header controls whether the network connection stays on after the current transaction finishes. If the value sent is keep-alive, the connection is persistent and not closed. Allowing for subsequent request to the same server to be done. | String | M
User-agent | The User-agent request header contains a characteristics string that allows the network protocol peers to identify the application type, operating system, software vendor of software version of the requesting software user agent. ALTOPay will send specific User-agent to merchant. | String | M
X-Alto-Timestamp | Timestamp in GMT sent by ALTOPay. This timestamp is required to generate signature. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSSZ | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M
Host | Host is the name of the server accessed by ALTOPay. From this header, the merchant server can find out how ALTOPay is connected to the merchant server. | String | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | Transaction command. | String | M
data | Container of the transaction data. | Object	
&nbsp;&nbsp;bank_code | Bank code from where ALTOPay get the payment request. <br> Can begin with 0. | Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number set by bank. | String(20) | M
&nbsp;&nbsp;pg_code | Payment gateway code on ALTOPay virtual account. <br> Can begin with 0. Each payment gateway will have an unique payment gateway code. The payment gateway just need to verify that the payment gateway is theirs.	| Numeric(2) | M
&nbsp;&nbsp;customer_number | Customer number. can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;customer_name | Customer name from the inquiry response. Some banks may be trimming the customer name due their system spec. Some banks also may not send the customer name. The merchant must check the customer name if exists. | String(50) | O
&nbsp;&nbsp;request_id | Request ID sent by ALTOPay. If the bank send request ID, ALTOPay will forward this request ID. If the bank does not send request ID, ALTOPay will generate a request ID. | String(32) | O
&nbsp;&nbsp;reference_number | Reference number of the payment request. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the payment. | Numeric(4) | O
&nbsp;&nbsp;date_time | Transmission date and time in GMT.  <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSS’Z’. | String(24) | M
&nbsp;&nbsp;total_amount | Total amount of all bills. It will be same with the total amount from the previous inquiry response. | Numeric | O
&nbsp;&nbsp;paid_amount | Total amount paid by customer for current transaction. | Numeric M
&nbsp;&nbsp;currency_code | Currency code of the transaction. If the currency code of the payment sent by ALTOPay is different with the currency code of the bill, the merchant needs to convert the amount to determine that the amount sent by the customer is enough to pay the bill. | String(3) | M
&nbsp;&nbsp;bill_list | Bill list. | Object	
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. <br> Can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;&nbsp;&nbsp;bill_amount | Amount paid by the customer for the bill. It can be different with the bill amount on the merchant. | Numeric | M
&nbsp;&nbsp;&nbsp;&nbsp;payment_reference | Payment reference from bank. Some banks do not send  the payment reference but ALTOPay will fill it with reference number if bank does not send payment reference. | String(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;repetition | The number of repetitions of check status request. It will be 1 on first check status, 2 on second check status, 3 on third check status, and so on. | Numeric | M


### Check Status Response Message

>Example Response Message

```http
HTTP/1.1 200 OK
X-Powered-By: PHP/5.6.35
Content-Type: application/json
Content-Length: 2515
```
```json
{
    "command": "check-status",
    "response_code": "00",
    "response_text": "Success",
    "message": {
        "id": "Sukses",
        "en": "Success"
    },
    "data": {
        "bank_code": "014",
        "virtual_account_number": "1234512371199991",
        "pg_code": "12",
        "customer_number": "71199991",
        "customer_name": "Mas Roy",
        "request_id": "12345",
        "reference_number": "000000000565",
        "channel_type": "6014",
        "time_stamp": "2019-07-12T04:23:14.000Z",
        "total_amount": 1500000,
        "paid_amount": 1500000,
        "currency_code": "IDR",
        "bill_list": [
            {
                "bill_number": "0001",
                "bill_amount": 1500000,
                "bill_description": {
                    "id": "Rock Pi 4",
                    "en": "Rock Pi 4"
                },
                "payment_status": "00",
                "payment_reference": "34264823420",
                "payment_message": {
                    "id": "Sukses",
                    "en": "Success"
                }
            }
        ]
    }
}
```

**Header**:

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | The transaction command that indicates the transaction. <br> Value: authentication-request-repeat. | String (30) | M
response_code | Payment response code indicates whether transaction is success or not. | String(3) | M
response_text | Response code description. | String(50) | M
message | Multilingual message to be displayed to the customer. | Multilingual object | M
data | Container of the transaction data. | Object	
&nbsp;&nbsp;bank_code | Bank code from where customer request inquiry. <br> Can begin with 0. | Numeric(3) | M
&nbsp;&nbsp;virtual_account_number | Full virtual account number sent by bank. | String(20) | M
&nbsp;&nbsp;pg_code | Payment gateway code on ALTOPay virtual account. can begin with 0. | Numeric(2) | M
&nbsp;&nbsp;customer_number | Customer number. can begin with 0. | Numeric(12) | M
&nbsp;&nbsp;customer_name | Customer name | Numeric(50) | M
&nbsp;&nbsp;request_id | Request ID sent by ALTOPay for current transaction. | String(32) | O
&nbsp;&nbsp;reference_number | Transaction reference number sent by ALTOPay for current transaction. | String(32) | M
&nbsp;&nbsp;channel_type | Channel type from which customer send the inquiry. | Numeric(4)	O
&nbsp;&nbsp;time_stamp| Response date and time in GMT. <br> Format: yyyy-mm-dd’T’hh:mm:ss.SSS’Z’. | String(24) | M
&nbsp;&nbsp;total_amount | Total amount of all bills sent by ALTOPay for current transaction. | Numeric | O
&nbsp;&nbsp;paid_amount | Total amount paid customer sent by ALTOPay for current transaction. | Numeric | M
&nbsp;&nbsp;currency_code | Currency code sent by ALTOPay for current transaction. | String(3) | M
&nbsp;&nbsp;bill_list | Bill list. | Object	
&nbsp;&nbsp;&nbsp;&nbsp;bill_amount | Bill amount. Mandatory if bill exists. | Numeric | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_number | Bill number. Mandatory if bill exists. <br> Can begin with 0. | Numeric(12) | C
&nbsp;&nbsp;&nbsp;&nbsp;bill_description | Bill description to be displayed to the customer. Mandatory if bill exists. | Multilingual object | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_status | Payment status. | String(3) | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_message | Multilingual message to be displayed to the customer. | Multilingual object | C
&nbsp;&nbsp;&nbsp;&nbsp;payment_rereference | Payment reference sent by ALTOPay if exist. | String(20) | C



## Merchant Registration 
Registration for a Merchant. (Example, New Registration, Update Data, and Upload Document.)

###List of Documents

No. | Document
----|---------
1. | Trade business license
2. | Taxpayer registration number
3. |Company’s deed
4. | Company registration certificate 
5. | Director nationality ID
6. | Decree of the minister of justice and human rights

### Host Merchant Registration
Setting | Value
--------|------
HTTP Method | POST
Path | /api/merchant/
Host-Sandbox | dashboard-sandbox.altopay.id

### Merchant Registration Request Message

> Example Request Message

```http
POST /api/merchant/ HTTP/1.1
Authorization: Basic dGlvQGFsdG8uaWQ6QnVuZ2ExMjMq
Content-type: application/json; charset="utf-8"
Accept-language: en-US, id-ID
Accept: application/json
Content-Length: 1154
```
```json
{
	"command":"merchant-registration",
	"data":{
		"merchant_name":"Mas Roy Pay",
		"merchant_code":"013",
		"company_name":"Royal Persada Nusantara",
		"company_address":"Jalan Bali Nomor 1",
		"company_phone":"081111111112",
		"company_email":"planet@kamspay.com",
		"company_taxpayer_id":"081266666666",
		"pic_name":"Kamshory",
		"pic_phone":"0811111111",
		"pic_email":"kamshory@domain.tld",
		"documents":[
			{
				"name":"TDP.jpg",
				"extension": "jpg",
				"mime_type": "image/jpeg",
				"document_type": "Image",
				"description":"Tanda Daftar Perusahaan",
				"data":"/9j/4AAQSkZJRgABAQEAYABgAAD/"
			},
			{
				"name":"Lorem Ipsum.txt",
				"extension": "txt",
				"mime_type": "text/plain",
				"document_type": "Text Document",
				"description":"Lorem Ipsum",
				"data":"TG9yZW0gaXBzdW0gZG9"
			}
		]
	}
}
```


**Header**:  

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Authorization | Auth: Basic Auth	| String | M
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Accept-language | The Accept-Language request HTTP header advertises which languages the client can understand, and which locale variant is preferred. (By languages, we mean natural languages, such as English, and not programming languages.) Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according to their user interface language and even if a user can change it, this happens rarely (and is frowned upon as it leads to fingerprinting). <br> This header is a hint to be used when the server has no way of determining the language via another way, like a specific URL, that is controlled by an explicit user decision. It is recommended that the server never overrides an explicit decision. The content of the Accept-Language is often out of the control of the user (like when traveling and using an Internet Cafe in a different country); the user may also want to visit a page in another language than the locale of their user interface. <br> If the server cannot serve any matching language, it can theoretically send back a 406 (Not Acceptable) error code. But, for a better user experience, this is rarely done, and more common way is to ignore the Accept-Language header in this case. | String | O
Accept | The accept request HTTP header advertises which content types, expressed as MIME types, the client can understand. Using content negotiation, the server then selects one of the proposals, use it and informs the client of its choice with the Content-type response header. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | Transaction command. | String | M
data | Container of the transaction data. | Object	
&nbsp;&nbsp;merchant_name | Merchant Name. | String(20) | M
&nbsp;&nbsp;merchant_code | Merchant Code. | String(20) | M
&nbsp;&nbsp;company_name | Company Name. | String(20)| M
&nbsp;&nbsp;company_address | Company Address. | String(20) | M
&nbsp;&nbsp;company_phone | Company Phone. | String(50) | O
&nbsp;&nbsp;company_email| Company Email. | String(32) | O
&nbsp;&nbsp;company_taxpayer_id | Company Taxprayer ID. | String(32) | M
&nbsp;&nbsp;pic_name | PIC Name. | String(32 | O
&nbsp;&nbsp;pic_phone | PIC Phone. | String(24) | M
&nbsp;&nbsp;pic_email | PIC Email. | String(24) | O
&nbsp;&nbsp;documents | Documents. | String(24) | M
&nbsp;&nbsp;&nbsp;&nbsp;name | Name. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;extention | Extention. | String(32) | M	
&nbsp;&nbsp;&nbsp;&nbsp;mime_type | Mime Type. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;document_type | Document Type. | String(32 | M
&nbsp;&nbsp;&nbsp;&nbsp;description | Description. | String(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;data | Data. | String(32) | O

### Merchant Registration Response Message

>Example Response Message

```http
HTTP/1.1 200 OK
X-Powered-By: PHP/5.6.35
Content-Type: application/json
Content-Length: 2515
```
```json
{
    "command": "merchant-registration",
    "response_code": "00",
    "response_text": "Success",
    "data": {
        "merchant_name": "Mas Roy Pay",
        "merchant_code": "013",
        "company_name": "Royal Persada Nusantara",
        "company_address": "Jalan Bali Nomor 1",
        "company_phone": "081111111112",
        "company_email": "planet@kamspay.com",
        "company_taxpayer_id": "081266666666",
        "pic_name": "Kamshory",
        "pic_phone": "0811111111",
        "pic_email": "kamshory@domain.tld",
        "documents": [
            {
                "name": "TDP.jpg",
                "extension": "jpg",
                "mime_type": "image/jpeg",
                "document_type": "Image",
                "description": "Tanda Daftar Perusahaan",
                "sha1_file": "04f52f24e6c9ed89537b491765ce6680c600442b",
                "saved": true,
                "exists": false
            },
            {
                "name": "Lorem Ipsum.txt",
                "extension": "txt",
                "mime_type": "text/plain",
                "document_type": "Text Document",
                "description": "Lorem Ipsum",
                "sha1_file": "404d3d9a19627d79923e0e5d70f5509b7fd0009a",
                "saved": true,
                "exists": false
            }
        ]
    }
}
```

**Header**:

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | The transaction command that indicates the transaction. <br> Value: authentication-request-repeat. | String (30) | M
response_code | Payment response code indicates whether transaction is success or not. | String(3) | M
response_text | Response code description. | String(50) | M
message | Multilingual message to be displayed to the customer.	| Multilingual object | M
data | Container of the transaction data.	| Object	
&nbsp;&nbsp;merchant_name | Merchant Name. | String(20) | M
&nbsp;&nbsp;merchant_code | Merchant Code. | String(20) | M
&nbsp;&nbsp;company_name | Company Name. | String(20)| M
&nbsp;&nbsp;company_address | Company Address. | String(20) | M
&nbsp;&nbsp;company_phone | Company Phone. | String(50) | O
&nbsp;&nbsp;company_email| Company Email. | String(32) | O
&nbsp;&nbsp;company_taxpayer_id | Company Taxprayer ID. | String(32) | M
&nbsp;&nbsp;pic_name | PIC Name. | String(32 | O
&nbsp;&nbsp;pic_phone | PIC Phone. | String(24) | M
&nbsp;&nbsp;pic_email | PIC Email. | String(24) | O
&nbsp;&nbsp;documents | Documents. | String(24) | M
&nbsp;&nbsp;&nbsp;&nbsp;name | Name. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;extention | Extention. | String(32) | M	
&nbsp;&nbsp;&nbsp;&nbsp;mime_type | Mime Type. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;document_type | Document Type. | String(32 | M
&nbsp;&nbsp;&nbsp;&nbsp;description | Description. | String(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;data | Data. | String(32) | O


## Merchant Info 
Information for each Merchant.


### Host Merchant Registration
Setting | Value
--------|------
HTTP Method | POST
Path | /api/merchant/
Host-Sandbox | dashboard-sandbox.altopay.id

### Merchant Registration Request Message

> Example Request Message

```http
POST /api/merchant/ HTTP/1.1
Authorization: Basic dGlvQGFsdG8uaWQ6QnVuZ2ExMjMq
Content-type: application/json; charset="utf-8"
Accept-language: en-US, id-ID
Accept: application/json
Content-Length: 1154
```
```json
{
	"command":"merchant-info",
	"data":{
		"merchant_code":"013"
	}
}
```


**Header**:  

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Authorization | Auth: Basic Auth	| String | M
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Accept-language | The Accept-Language request HTTP header advertises which languages the client can understand, and which locale variant is preferred. (By languages, we mean natural languages, such as English, and not programming languages.) Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according to their user interface language and even if a user can change it, this happens rarely (and is frowned upon as it leads to fingerprinting). <br> This header is a hint to be used when the server has no way of determining the language via another way, like a specific URL, that is controlled by an explicit user decision. It is recommended that the server never overrides an explicit decision. The content of the Accept-Language is often out of the control of the user (like when traveling and using an Internet Cafe in a different country); the user may also want to visit a page in another language than the locale of their user interface. <br> If the server cannot serve any matching language, it can theoretically send back a 406 (Not Acceptable) error code. But, for a better user experience, this is rarely done, and more common way is to ignore the Accept-Language header in this case. | String | O
Accept | The accept request HTTP header advertises which content types, expressed as MIME types, the client can understand. Using content negotiation, the server then selects one of the proposals, use it and informs the client of its choice with the Content-type response header. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | Transaction command. | String | M
data | Container of the transaction data. | Object	
&nbsp;&nbsp;merchant_code | Merchant Code. | String(20) | M

### Merchant Registration Response Message

>Example Response Message

```http
HTTP/1.1 200 OK
X-Powered-By: PHP/5.6.35
Content-Type: application/json
Content-Length: 2515
```
```json
{
    "command": "merchant-info",
    "response_code": "00",
    "response_text": "Success",
    "data": {
        "merchant_name": "Mas Roy Pay",
        "merchant_code": "013",
        "company_name": "Royal Persada Nusantara",
        "company_address": "Jalan Bali Nomor 1",
        "company_phone": "081111111112",
        "company_email": "planet@kamspay.com",
        "company_taxpayer_id": "081266666666",
        "pic_name": "Kamshory",
        "pic_phone": "0811111111",
        "pic_email": "kamshory@domain.tld",
        "registration_time": "2020-04-06T10:53:11.000Z",
        "last_update_time": "2020-04-07T03:15:03.000Z",
        "documents": [
            {
                "name": "TDP",
                "extention": "jpg",
                "mime_type": "image/jpeg",
                "document_type": "Image",
                "description": "Tanda Daftar Perusahaan",
                "file_size": 101654,
                "hash_sha1": "a29bfa4f55a8419f4729d36718f857d44355c01b",
                "upload_time": "2020-04-06T10:53:11.000Z"
            },
            {
                "name": "Lorem Ipsum",
                "extention": "txt",
                "mime_type": "text/plain",
                "document_type": "Text Document",
                "description": "Lorem Ipsum",
                "file_size": 2611,
                "hash_sha1": "792824f58f8b77c8f3c1ffeb7999c6c232464452",
                "upload_time": "2020-04-06T10:53:11.000Z"
            },
            {
                "name": "TDP",
                "extention": "jpg",
                "mime_type": "image/jpeg",
                "document_type": "Image",
                "description": "Tanda Daftar Perusahaan",
                "file_size": 21,
                "hash_sha1": "04f52f24e6c9ed89537b491765ce6680c600442b",
                "upload_time": "2020-04-07T03:11:14.000Z"
            },
            {
                "name": "Lorem Ipsum",
                "extention": "txt",
                "mime_type": "text/plain",
                "document_type": "Text Document",
                "description": "Lorem Ipsum",
                "file_size": 14,
                "hash_sha1": "404d3d9a19627d79923e0e5d70f5509b7fd0009a",
                "upload_time": "2020-04-07T03:11:14.000Z"
            }
        ]
    }
}
```

**Header**:

Parameter | Description | Data Type	| Type
----------|-------------|-----------|-----
Content-type | Format for data sent by ALTOPay. Format of the data sent is application/json with chatset utf-8. | String | M
Content-length | The Content-length entity header indicates the size of the entity-body, in bytes, sent to merchant. | Numeric | M

**Body**:  

Parameter | Description	| Data Type | Type
----------|-------------|-----------|-----
command | The transaction command that indicates the transaction. <br> Value: authentication-request-repeat. | String (30) | M
response_code | Payment response code indicates whether transaction is success or not. | String(3) | M
response_text | Response code description. | String(50) | M
message | Multilingual message to be displayed to the customer.	| Multilingual object | M
data | Container of the transaction data.	| Object	
&nbsp;&nbsp;merchant_name | Merchant Name. | String(20) | M
&nbsp;&nbsp;merchant_code | Merchant Code. | String(20) | M
&nbsp;&nbsp;company_name | Company Name. | String(20)| M
&nbsp;&nbsp;company_address | Company Address. | String(20) | M
&nbsp;&nbsp;company_phone | Company Phone. | String(50) | O
&nbsp;&nbsp;company_email| Company Email. | String(32) | O
&nbsp;&nbsp;company_taxpayer_id | Company Taxprayer ID. | String(32) | M
&nbsp;&nbsp;pic_name | PIC Name. | String(32 | O
&nbsp;&nbsp;pic_phone | PIC Phone. | String(24) | M
&nbsp;&nbsp;pic_email | PIC Email. | String(24) | O
&nbsp;&nbsp;documents | Documents. | String(24) | M
&nbsp;&nbsp;&nbsp;&nbsp;name | Name. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;extention | Extention. | String(32) | M	
&nbsp;&nbsp;&nbsp;&nbsp;mime_type | Mime Type. | String(32) | M
&nbsp;&nbsp;&nbsp;&nbsp;document_type | Document Type. | String(32 | M
&nbsp;&nbsp;&nbsp;&nbsp;description | Description. | String(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;file_size | File Size. | Numeric(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;hash_sha1 | HASH SHA1. | String(32) | O
&nbsp;&nbsp;&nbsp;&nbsp;data | Data. | String(32) | O
