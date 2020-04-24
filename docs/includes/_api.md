# Product Message Format

Each product has a different message format depending on each biller. AltoPay adjusts the message format for each product according to its needs. Some products have far more data attributes than other products because they are related to biller policy.

Some products that have the same attributes and processes are combined into one product category, namely general billing. Some more products are made with special categories because they have very big differences.

Every message is always listed one of _product_code_ and _product_type. product_type_ is only used for prepaid airtime topup and postpaid airtime bill payments.

## Prepaid Airtime

### Payment Request

**Message Sample**
```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 21834cad678df58049af0a0b17d44d850a84f0525e4443e41e9385a4208ceb1e
Content-length: 200

{
	"command": "payment",
	"product_type": "1010",
	"data":{
		"date_time": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"customer_id": "08123456789",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Data Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String |  | M | Transaction command |
| 2 | product_type | Numeric, can begin with 0 |  | M | Product type |
| 3 | `data`.date_time | String | 23 | M | Response time stamp |
| 4 | `data`.reference_number | String | 20 | M | Reference number |
| 5 | `data`.customer_id | String | 12 | M | Customer ID |
| 6 | `data`.amount | Decimal | 12 | M | Amount |
| 7 | `data`.merchant_type | Numeric | 4 | O | Merchant type of the channel where customer pay the bill |
| 8 | `data`.locket_code | String | 32 | O | Locket code where customer pay |
| 9 | `data`.locket_name | String | 30 | O | Locket name where customer pay |
| 10 | `data`.locket_address | String | 50 | O | Locket address where customer pay |
| 11 | `data`.locket_phone | String | 18 | O | Locket phone where customer pay |

### Payment Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 336

{
	"command": "payment",
	"product_type": "1010",
	"response_code": "001",
	"response_text": "Success",
	"command": "payment",
	"data":{
		"time_stamp": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"fwd_reference_number": "99999",
		"fwd_stan": "556287",
		"customer_id": "08123456789",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String | | Transaction command |
| 2 | product_type | String | | Product type |
| 3 | product_code | Numeric, can begin with 0 | | Product code |
| 4 | response_code | String | 3 | Response code |
| 5 | response_text | String | | Response text |
| 6 | `data`.time_stamp | String | 23 | Response time stamp |
| 7 | `data`.reference_number | String | 20 | Reference number |
| 8 | `data`.customer_id | String | 12 | Customer ID |
| 9 | `data`.amount | Decimal | 12 | Amount |
| 10 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 11 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan |

## Postpaid Airtime

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z.000Z
X-Alto-Signature: f3571b283dfd6579e9208c25e24a19e07941dae1e0c5b11d905ece2f34ec8585
Content-length: 176

{
	"command": "inquiry",
	"product_type": "1008",
	"data": {
		"reference_number": "1566985456",
		"date_time": "2019-08-28T16:44:16.000Z",
		"customer_id": "0811000000"
	}
}
```

**Field Description**

| No | Parameter | Data Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String |  | M | Transaction command |
| 2 | product_type | Numeric, can begin with 0 |  | M | Product type |
| 3 | `data`.date_time | String | 23 | M | Response time stamp |
| 4 | `data`.reference_number | String | 20 | M | Reference number |
| 5 | `data`.customer_id | String | 12 | M | Customer ID |


### Inquiry Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 363

{
	"response_code": "001",
	"biller_message": "Sukses",
	"data": {
		"amount": "235000",
		"time_stamp": "2019-08-28T09:44:18.650Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "0811000000",
		"reference_number": "1566985456"
	},
	"biller_response": "Sukses",
	"response_text": "Success",
	"product_code": "00800080001",
	"command": "inquiry"
}

```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String | | Transaction command |
| 2 | product_type | String | | Product type |
| 3 | product_code | Numeric, can begin with 0 | | Product code |
| 4 | response_code | String | 3 | Response code |
| 5 | response_text | String | | Response text |
| 6 | `data`.date_time | String | 23 | Response time stamp |
| 7 | `data`.reference_number | String | 20 | Reference number |
| 8 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 9 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 10 | `data`.customer_id | String | 12 | Customer ID |
| 11 | `data`.customer_name | String | 40 | Customer Name |
| 12 | `data`.amount | Decimal | 12 | Amount |

### Payment Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 8a3f728a8b2ae70e80b46742c74dc9bad45dea8a9671bb7b2e403c6ba4cca125
Content-length: 296

{
	"command": "payment",
	"product_type": "1008",
	"data": {
		"locket_address": "Jalan Anggrek Neli Murni",
		"currency_code": "IDR",
		"locket_name": "ALTO",
		"merchant_type": "6021",
		"locket_code": "123",
		"reference_number": "1566985456",
		"locket_phone": "02199999",
		"date_time": "2019-08-28T16:46:08.000Z",
		"amount": "235000",
		"time_stamp": "2019-08-28T09:44:18.650Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "0811000000"
	}
}
```

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_type | String | | M | Product type |
| 3 | product_code | Numeric, can begin with 0 | | M | Product code |
| 4 | `data`.date_time | String | 23 | M | Response time stamp |
| 5 | `data`.reference_number | String | 20 | M | Reference number |
| 6 | `data`.fwd_reference_number | String | 32 | M | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 7 | `data`.fwd_stan | Numeric | 6 | M | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 8 | `data`.customer_id | String | 12 | M | Customer ID |
| 9 | `data`.customer_name | String | 40 | M | Customer Name |
| 10 | `data`.amount | Decimal | 12 | M | Amount |
| 11 | `data`.merchant_type | Numeric | 4 | O | Merchant type of the channel where customer pay the bill |
| 12 | `data`.locket_code | String | 32 | O | Locket code where customer pay |
| 13 | `data`.locket_name | String | 30 | O | Locket name where customer pay |
| 14 | `data`.locket_address | String | 50 | O | Locket address where customer pay |
| 15 | `data`.locket_phone | String | 18 | O | Locket phone where customer pay |

### Payment Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 363

{
	"response_code": "001",
	"biller_message": "Sukses",
	"data": {
		"amount": "235000",
		"time_stamp": "2019-08-28T09:46:09.458Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "0811000000",
		"reference_number": "1566985456"
	},
	"biller_response": "Sukses",
	"response_text": "Success",
	"product_code": "00800080001",
	"command": "payment"
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String |  | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | Product code |
| 3 | response_code | String | | Response code |
| 4 | response_text | String | | Response text |
| 5 | `data`.date_time | String | 23 | Response time stamp |
| 6 | `data`.reference_number | String | 32 | Reference number |
| 7 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 8 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 9 | `data`.customer_id | String | 12 | Customer ID |
| 10 | `data`.customer_name | String | 40 | Customer Name |
| 11 | `data`.amount | Decimal | 12 | Amount |

## Moble Data

### Payment Request

**Message Sample**
```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 21834cad678df58049af0a0b17d44d850a84f0525e4443e41e9385a4208ceb1e
Content-length: 200

{
	"command": "payment",
	"product_type": "1011",
	"data":{
		"date_time": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"customer_id": "08123456789",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String |  | M | Transaction command |
| 2 | product_type | Numeric, can begin with 0 |  | M | Product type |
| 3 | `data`.date_time | String | 23 | M | Response time stamp |
| 4 | `data`.reference_number | String | 20 | M | Reference number |
| 5 | `data`.customer_id | String | 12 | M | Customer ID |
| 6 | `data`.amount | Decimal | 12 | M | Amount |
| 7 | `data`.merchant_type | Numeric | 4 | O | Merchant type of the channel where customer pay the bill |
| 8 | `data`.locket_code | String | 32 | O | Locket code where customer pay |
| 9 | `data`.locket_name | String | 30 | O | Locket name where customer pay |
| 10 | `data`.locket_address | String | 50 | O | Locket address where customer pay |
| 11 | `data`.locket_phone | String | 18 | O | Locket phone where customer pay |

### Payment Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 336

{
	"command": "payment",
	"product_type": "1011",
	"response_code": "001",
	"response_text": "Success",
	"command": "payment",
	"data":{
		"time_stamp": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"fwd_reference_number": "99999",
		"fwd_stan": "556287",
		"customer_id": "08123456789",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String | | Transaction command |
| 2 | product_type | String | | Product type |
| 3 | product_code | Numeric, can begin with 0 | | Product code |
| 4 | response_code | String | 3 | Response code |
| 5 | response_text | String | | Response text |
| 6 | `data`.date_time | String | 23 | Response time stamp |
| 7 | `data`.reference_number | String | 20 | Reference number |
| 8 | `data`.fwd_reference_number | String | 20 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 9 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 10 | `data`.customer_id | String | 12 | Customer ID |
| 11 | `data`.amount | Decimal | 12 | Amount |


## Prepaid Electricity

Prepaid electricity is a product of the _Perusahaan Listrik Negara_ (PLN) where customers can use electricity in accordance with the quota that has been purchased. By purchasing a power quota, customers will get a token that can be entered into the power controller.

The customer must enter the customer ID or meter number at the time of purchase because the generated token will be adjusted to the customer's meter number.

To prevent mistakes when purchasing, PLN requires customers to conduct an inquiry to confirm whether the customer number or meter number entered is correct. **All attributes on request message are mandatory**.

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: 769ab57e146beaefa1f0536f230e2ca0a86bafbe4186a8c9b2fe69fdac1f2026
Content-length: 346

{
    "command": "inquiry",
    "product_code": "053502",
    "data": {
        "channel_type": "6024",
        "currency_code": "IDR",
        "customer_id": "12345678901",
        "customer_reference_id_1": "0",
        "date_time": "2019-08-08T23:56:59.987Z",
        "reference_number": "123456789991",
        "total_admin_fee": 2500
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | 6 | M | Product Code |
| 3 | data.channel_type | String | 4 | M | Channel Type |
| 4 | data.currency_code | String | 3 | M | Currency Code |
| 5 | data.customer_id | String | 11 | M | Customer Id |
| 6 | data.customer_reference_id_1 | String | 1 | M | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 7 | data.date_time | String | 24 | M | Date Time |
| 8 | data.reference_number | String | 32 | M | Reference Number |
| 9 | data.total_admin_fee | Numeric | | M | Total Admin Fee |

### Inquiry Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 1899

{
    "biller_message": "",
    "biller_response": "",
    "command": "inquiry",
    "product_code": "053502",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 0,
        "balance_after": 1921049670,
        "balance_before": 1921049770,
        "bill_reference_id": "",
        "biller_aggregator_id": "360003",
        "biller_aggregator_switch_id": "360003",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "149345893458",
        "currency_code": "IDR",
        "customer_id": "149345893458",
        "customer_name": "AMING SUKARMIN BIN SANUSI",
        "customer_reference_id_1": "0",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320212165908",
        "fwd_stan": "749075",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "reference_number": "123456789991",
        "screen_admin_fee": "Rp 1.500",
        "screen_amount": "Rp 0",
        "screen_customer_id": "149345893458",
        "screen_customer_name": "AMING SUKARMIN BIN SANUSI",
        "screen_header": "PEMBELIAN PLN PRABAYAR",
        "screen_product_name": "PLN PREPAID",
        "screen_reserved_1": "",
        "screen_tariff": "B1 \/000001300 VA",
        "screen_token_unsold_1": "Rp 0",
        "screen_token_unsold_2": "Rp 0",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 0,
        "screen": [
            "PEMBELIAN PLN PRABAYAR",
            "",
            "BILLER : PLN PREPAID",
            "IDPEL : 149345893458",
            "NAMA  : AMING SUKARMIN BIN SANUSI",
            "TARIF\/DAYA : B1 \/000001300 VA",
            "TKN UNSOLD1 : Rp 0",
            "TKN UNSOLD2 : Rp 0",
            "NOMINAL : Rp 0",
            "ADMIN : Rp 1.500"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String | 6 | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 6 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 12 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 12 | Customer Id |
| 18 | data.customer_name | String | 34 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.reference_number | String | 32 | Reference Number |
| 27 | data.screen_admin_fee | String | | Screen Admin Fee |
| 28 | data.screen_amount | String | | Screen Amount |
| 29 | data.screen_customer_id | String | | Screen Customer Id |
| 30 | data.screen_customer_name | String | | Screen Customer Name |
| 31 | data.screen_header | String | | Screen Header |
| 32 | data.screen_product_name | String | | Screen Product Name |
| 33 | data.screen_reserved_1 | String | | Screen Reserved 1 |
| 34 | data.screen_tariff | String | | Screen Tariff |
| 35 | data.screen_token_unsold_1 | String | | Screen Token Unsold 1 |
| 36 | data.screen_token_unsold_2 | String | | Screen Token Unsold 2 |
| 37 | data.time_stamp | String | 24 | Time Stamp |
| 38 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 39 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 40 | data.screen | Array |  | Screen |

### Purchase Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 643

{
    "command": "payment",
    "product_code": "053502",
    "data": {
        "amount": 50000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "149345893458",
        "customer_name": "AMING SUKARMIN BIN SANUSI",
        "customer_reference_id_1": "0",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211105739",
        "fwd_stan": "860601",
        "payment_mode": "12",
        "reference_number": "123456789991",
        "total_admin_fee": 2500,
        "total_bill_amount": 50000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 12 | M | Customer Id |
| 8 | data.customer_name | String | 34 | M | Customer Name |
| 9 | data.customer_reference_id_1 | String | 1 | M | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 10 | data.customer_reference_id_2 | String | | M | Customer Reference Id 2 |
| 11 | data.date_time | String | 24 | M | Date Time |
| 12 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 13 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 14 | data.payment_mode | String | 2 | M | Payment Mode |
| 15 | data.reference_number | String | 32 | M | Reference Number |
| 16 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 17 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Purchase Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2641

{
    "biller_message": "",
    "biller_response": "",
    "command": "payment",
    "product_code": "053502",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 50000,
        "balance_after": 1920945670,
        "balance_before": 1920997670,
        "bill_reference_id": "",
        "biller_aggregator_id": "",
        "biller_aggregator_switch_id": "360003",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "149345893458",
        "currency_code": "IDR",
        "customer_id": "149345893458",
        "customer_name": "AMING SUKARMIN BIN SANUSI",
        "customer_reference_id_1": "0",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211105739",
        "fwd_stan": "860601",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_total_admin_fee": "Rp 2.500",
        "receipt_amount": "Rp 50.000",
        "receipt_ceiling": "61,0",
        "receipt_customer_id": "149345893458",
        "receipt_customer_name": "AMING SUKARMIN BIN SANUSI",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA(PLN)",
        "receipt_info_text_2": " MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": " BUKTI PEMBAYARAN YANG SAH",
        "receipt_meter_number": "10077764594",
        "receipt_paid_amount": "Rp 52.500",
        "receipt_ppj": "Rp 1.200,00",
        "receipt_ppn": "Rp 0,00",
        "receipt_product_name": "PLN PREPAID",
        "receipt_reference_number_1": "",
        "receipt_stamp": "Rp 0,00",
        "receipt_tariff": "B1 \/000001300 VA",
        "receipt_token": "4138 0001 5718 1799 3532",
        "receipt_total_amount": "Rp 52.500",
        "reference_number": "123456789991",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 50000,
        "receipt": [
            "BILLER : PLN PREPAID",
            "ID PLNGGAN : 149345893458",
            "NAMA : AMING SUKARMIN BIN SANUSI",
            "TARIF\/DAYA : B1 \/000001300 VA",
            "JML KWH : 61,0",
            "NO METER : 10077764594",
            "NO REF : 0UAK241100000000E48A",
            "",
            "NOMINAL : Rp 50.000",
            "ADMIN : Rp 2.500",
            "TOTAL BAYAR: Rp 52.500",
            "TOKEN : 4138 0001 5718 1799 3532",
            "RP BAYAR: Rp 52.500",
            "METERAI : Rp 0,00",
            "PPN : Rp 0,00",
            "PPJ : Rp 1.200,00",
            "PERUSAHAAN LISTRIK NEGARA(PLN)",
            " MENYATAKAN STRUK INI SEBAGAI",
            " BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String |  | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 6 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 12 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 12 | Customer Id |
| 18 | data.customer_name | String | 34 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.receipt_total_admin_fee | String | | Receipt Total Admin Fee |
| 27 | data.receipt_amount | String | | Receipt Amount |
| 28 | data.receipt_ceiling | String | | Receipt Ceiling |
| 29 | data.receipt_customer_id | String | | Receipt Customer Id |
| 30 | data.receipt_customer_name | String | | Receipt Customer Name |
| 31 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 32 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 33 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 34 | data.receipt_meter_number | String | | Receipt Meter Number |
| 35 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 36 | data.receipt_ppj | String | | Receipt Ppj |
| 37 | data.receipt_ppn | String | | Receipt Ppn |
| 38 | data.receipt_product_name | String | | Receipt Product Name |
| 39 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 40 | data.receipt_stamp | String | | Receipt Stamp |
| 41 | data.receipt_tariff | String | | Receipt Tariff |
| 42 | data.receipt_token | String | | Receipt Token |
| 43 | data.receipt_total_amount | String | | Receipt Total Amount |
| 44 | data.reference_number | String | 32 | Reference Number |
| 45 | data.time_stamp | String | 24 | Time Stamp |
| 46 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 47 | data.total_bill_amount | Numeric | | Total Admin Fee |
| 48 | data.receipt | Array | | Receipt |

### Prepaid Advice Request

If the connection is lost when the token has been generated by the vending machine but has not been received by the customer, the customer can request the token again by sending a advice message. The same thing happens if the token has been received by the customer but is lost while the token has not been entered into the power controller.

To request a return token that has been purchased, the customer must send a advice message. The parameters of the advice message are as follows:
1. `customer_id`
2. `meter_id`
3. `id_selector`
4. `adv_reference_number`

`adv_reference_number` is the reference number that was sent when making a purchase.
If the customer does not know the reference number, AltoPay Biller will select the last transaction and include all reference numbers and transaction times on that day. Customers can choose transactions from the options provided if the requested token is not the last token of the day.

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 642

{
    "command": "advice",
    "product_code": "053502",
    "data": {
        "amount": 50000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "149345893458",
        "customer_name": "AMING SUKARMIN BIN SANUSI",
        "customer_reference_id_1": "0",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211105739",
        "fwd_stan": "860601",
        "payment_mode": "12",
        "reference_number": "123456789991",
        "total_admin_fee": 2500,
        "total_bill_amount ": 50000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 12 | M | Customer Id |
| 8 | data.customer_name | String | 34 | M | Customer Name |
| 9 | data.customer_reference_id_1 | String | 1 | M | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 10 | data.customer_reference_id_2 | String | | M | Customer Reference Id 2 |
| 11 | data.date_time | String | 24 | M | Date Time |
| 12 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 13 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 14 | data.payment_mode | String | 2 | M | Payment Mode |
| 15 | data.reference_number | String | 32 | M | Reference Number |
| 16 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 17 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Prepaid Advice Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2639

{
    "biller_message": "",
    "biller_response": "",
    "command": "payment",
    "product_code": "053502",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 50000,
        "balance_after": 1920893670,
        "balance_before": 1920945670,
        "bill_reference_id": "",
        "biller_aggregator_id": "",
        "biller_aggregator_switch_id": "360003",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "149345893458",
        "currency_code": "IDR",
        "customer_id": "149345893458",
        "customer_name": "AMING SUKARMIN BIN SANUSI",
        "customer_reference_id_1": "0",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211105739",
        "fwd_stan": "860601",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_total_admin_fee": "Rp 2.500",
        "receipt_amount": "Rp 50.000",
        "receipt_ceiling": "61,0",
        "receipt_customer_id": "149345893458",
        "receipt_customer_name": "AMING SUKARMIN BIN SANUSI",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA(PLN)",
        "receipt_info_text_2": " MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": " BUKTI PEMBAYARAN YANG SAH",
        "receipt_meter_number": "10077764594",
        "receipt_paid_amount": "Rp 52.500",
        "receipt_ppj": "Rp 1.200,00",
        "receipt_ppn": "Rp 0,00",
        "receipt_product_name": "PLN PREPAID",
        "receipt_reference_number_1": "",
        "receipt_stamp": "Rp 0,00",
        "receipt_tariff": "B1 \/000001300 VA",
        "receipt_token": "4138 0001 5718 1799 3532",
        "receipt_total_amount": "Rp 52.500",
        "reference_number": "123456789991",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 50000,
        "receipt": [
            "BILLER : PLN PREPAID",
            "ID PLNGGAN : 149345893458",
            "NAMA : AMING SUKARMIN BIN SANUSI",
            "TARIF\/DAYA : B1 \/000001300 VA",
            "JML KWH : 61,0",
            "NO METER : 10077764594",
            "NO REF : 0UAK241100000000E48A",
            "",
            "NOMINAL : Rp 50.000",
            "ADMIN : Rp 2.500",
            "TOTAL BAYAR: Rp 52.500",
            "TOKEN : 4138 0001 5718 1799 3532",
            "RP BAYAR: Rp 52.500",
            "METERAI : Rp 0,00",
            "PPN : Rp 0,00",
            "PPJ : Rp 1.200,00",
            "PERUSAHAAN LISTRIK NEGARA(PLN)",
            " MENYATAKAN STRUK INI SEBAGAI",
            " BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String |  | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 6 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 12 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 12 | Customer Id |
| 18 | data.customer_name | String | 34 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.receipt_total_admin_fee | String | | Receipt Total Admin Fee |
| 27 | data.receipt_amount | String | | Receipt Amount |
| 28 | data.receipt_ceiling | String | | Receipt Ceiling |
| 29 | data.receipt_customer_id | String | | Receipt Customer Id |
| 30 | data.receipt_customer_name | String | | Receipt Customer Name |
| 31 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 32 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 33 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 34 | data.receipt_meter_number | String | | Receipt Meter Number |
| 35 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 36 | data.receipt_ppj | String | | Receipt Ppj |
| 37 | data.receipt_ppn | String | | Receipt Ppn |
| 38 | data.receipt_product_name | String | | Receipt Product Name |
| 39 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 40 | data.receipt_stamp | String | | Receipt Stamp |
| 41 | data.receipt_tariff | String | | Receipt Tariff |
| 42 | data.receipt_token | String | | Receipt Token |
| 43 | data.receipt_total_amount | String | | Receipt Total Amount |
| 44 | data.reference_number | String | 32 | Reference Number |
| 45 | data.time_stamp | String | 24 | Time Stamp |
| 46 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 47 | data.total_bill_amount | Numeric | Total Bill Amount |
| 48 | data.receipt | Array | | Receipt |


## Postpaid Electricity

Postpaid Electricity is the payment of postpaid electricity bills from the Perusahaan Listrik Negara (PLN). PLN requires an inquiry before making a payment to ensure that the customer number is entered correctly and also to see how much the electricity bill for the month.

PLN requires agents to enter the total admin fee paid by the customer. Therefore, it is not possible for agents to take admin fees outside of the provisions. **All the attributes in the request message are mandarory**.

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 306

{
    "command": "inquiry",
    "product_code": "054501",
    "data": {
        "channel_type": "6024",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "date_time": "2019-08-08T23:56:59.987Z",
        "reference_number": "12345678900",
        "total_admin_fee": 2500
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.channel_type | String | 4 | M | Channel Type |
| 4 | data.currency_code | String | 3 | M | Currency Code |
| 5 | data.customer_id | String | 12 | M | Customer Id |
| 6 | data.date_time | String | 24 | M | Date Time |
| 7 | data.reference_number | String | 11 | M | Reference Number |
| 8 | data.total_admin_fee | Numeric | | M | Total Admin Fee |

### Inquiry Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 1859

{
    "biller_message": "",
    "biller_response": "",
    "command": "inquiry",
    "product_code": "054501",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 700000,
        "balance_after": 1920893370,
        "balance_before": 1920893470,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "3",
        "contract_number": "541100064594",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320212171145",
        "fwd_stan": "756653",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "reference_number": "12345678900",
        "screen_admin_fee": "2.500",
        "screen_bill_amount": "700.000",
        "screen_bill_count": "3 BULAN",
        "screen_bill_period": "AN15,FEB15,MAR15",
        "screen_customer_id": "541100064591",
        "screen_customer_name": "ACENG SURACENG",
        "screen_header": "INFORMASI TAGIHAN PLN",
        "screen_paid_amount": "702.500",
        "screen_product_name": "PLN POSTPAID",
        "screen_reserved_1": "",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000,
        "screen": [
            "INFORMASI TAGIHAN PLN",
            "",
            "BILLER : PLN POSTPAID",
            "IDPEL : 541100064591",
            "NAMA  : ACENG SURACENG",
            "JBL\/TH : AN15,FEB15,MAR15",
            "TOTAL TGHN : 3 BULAN",
            "RP TAG PLN : 700.000",
            "ADMIN BANK: 2.500",
            "TOTAL BAYAR : 702.500"
        ]
    }
}
```


**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 12 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 12 | Customer Id |
| 18 | data.customer_name | String | 14 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.reference_number | String | 11 | Reference Number |
| 27 | data.screen_admin_fee | String | | Screen Admin Fee |
| 28 | data.screen_bill_amount | String | | Screen Bill Amount |
| 29 | data.screen_bill_count | String | | Screen Bill Count |
| 30 | data.screen_bill_period | String | | Screen Bill Period |
| 31 | data.screen_customer_id | String | | Screen Customer Id |
| 32 | data.screen_customer_name | String | | Screen Customer Name |
| 33 | data.screen_header | String | | Screen Header |
| 34 | data.screen_paid_amount | String | | Screen Paid Amount |
| 35 | data.screen_product_name | String | | Screen Product Name |
| 36 | data.screen_reserved_1 | String |  | Screen Reserved 1 |
| 37 | data.time_stamp | String | 24 | Time Stamp |
| 38 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 38 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 39 | data.screen | Array |  | Screen |

### Payment Request

**Message Sample**
```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 517

{
    "command": "payment",
    "product_code": "054501",
    "data": {
        "amount": 700000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "payment_mode": "12",
        "reference_number": "12345678902",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 12 | M | Customer Id |
| 8 | data.customer_name | String | 14 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 11 | M | Reference Number |
| 14 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 15 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Payment Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2598

{
    "biller_message": "",
    "biller_response": "",
    "command": "payment",
    "product_code": "054501",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 700000,
        "balance_after": 1918085370,
        "balance_before": 1918787370,
        "bill_reference_id": "",
        "biller_aggregator_id": "",
        "biller_aggregator_switch_id": "360003",
        "channel_type": "6024",
        "component_of_bill": "3",
        "contract_number": "541100064594",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "700.000",
        "receipt_bill_count": "3 BULAN",
        "receipt_customer_id": "541100064591",
        "receipt_customer_name": "ACENG SURACENG",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA (PLN)",
        "receipt_info_text_2": "MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": "BUKTI PEMBAYARAN YANG SAH",
        "receipt_meter_number": "",
        "receipt_paid_amount": "702.500",
        "receipt_penalty": "",
        "receipt_period_id": "JAN15,FEB15,MAR15",
        "receipt_product_name": "PLN POSTPAID",
        "receipt_reference_number_1": "0UAK24110000000086CA",
        "receipt_reference_number_2": "",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_stand_begin_end": "00400830-00631144",
        "receipt_tariff": "R3 \/000002200 VA",
        "reference_number": "12345678902",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000,
        "receipt": [
            "BILLER : PLN POSTPAID",
            "IDPEL : 541100064591",
            "NAMA : ACENG SURACENG",
            "BL\/TH : JAN15,FEB15,MAR15",
            "STAND METER: 00400830-00631144",
            "",
            "TOTAL TGHN : 3 BULAN",
            "TARIF\/DAYA : R3 \/000002200 VA",
            "NO REF : 0UAK24110000000086CA",
            "",
            "RP TAG PLN : 700.000",
            "",
            "ADMIN BANK : 2.500",
            "TOTAL BAYAR : 702.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA (PLN)",
            "MENYATAKAN STRUK INI SEBAGAI",
            "BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String |  | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 6 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 12 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 12 | Customer Id |
| 18 | data.customer_name | String | 14 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 27 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 28 | data.receipt_bill_count | String | | Receipt Bill Count |
| 29 | data.receipt_customer_id | String | | Receipt Customer Id |
| 30 | data.receipt_customer_name | String | | Receipt Customer Name |
| 31 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 32 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 33 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 34 | data.receipt_meter_number | String | | Receipt Meter Number |
| 35 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 36 | data.receipt_penalty | String | | Receipt Penalty |
| 37 | data.receipt_period_id | String | | Receipt Period Id |
| 38 | data.receipt_product_name | String | | Receipt Product Name |
| 39 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 40 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 41 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 42 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 43 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 44 | data.receipt_tariff | String | | Receipt Tariff |
| 45 | data.reference_number | String | 11 | Reference Number |
| 46 | data.time_stamp | String | 24 | Time Stamp |
| 47 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 48 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 49 | data.receipt | Array | | Receipt |

### Advice Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 516

{
    "command": "advice",
    "product_code": "054501",
    "data": {
        "amount": 700000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "payment_mode": "12",
        "reference_number": "12345678902",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 12 | M | Customer Id |
| 8 | data.customer_name | String | 14 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 11 | M | Reference Number |
| 14 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 15 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Advice Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2476

{
    "biller_message": "",
    "biller_response": "",
    "command": "advice",
    "product_code": "054501",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 700000,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "3",
        "contract_number": "541100064594",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "700.000",
        "receipt_bill_count": "3 BULAN",
        "receipt_customer_id": "541100064591",
        "receipt_customer_name": "ACENG SURACENG",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA (PLN)",
        "receipt_info_text_2": "MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": "BUKTI PEMBAYARAN YANG SAH",
        "receipt_meter_number": "",
        "receipt_paid_amount": "702.500",
        "receipt_penalty": "",
        "receipt_period_id": "JAN15,FEB15,MAR15",
        "receipt_product_name": "PLN POSTPAID",
        "receipt_reference_number_1": "0UAK24110000000086CA",
        "receipt_reference_number_2": "",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_stand_begin_end": "00400830-00631144",
        "receipt_tariff": "R3 \/000002200 VA",
        "reference_number": "12345678902",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000,
        "receipt": [
            "BILLER : PLN POSTPAID",
            "IDPEL : 541100064591",
            "NAMA : ACENG SURACENG",
            "BL\/TH : JAN15,FEB15,MAR15",
            "STAND METER : 00400830-00631144",
            "",
            "TOTAL TGHN : 3 BULAN",
            "TARIF\/DAYA : R3 \/000002200 VA",
            "0UAK24110000000086CA",
            "",
            "RP TAG PLN : 700.000",
            "",
            "ADMIN BANK : 2.500",
            "TOTAL BAYAR : 702.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA (PLN)",
            "MENYATAKAN STRUK INI SEBAGAI",
            "BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```


**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.bill_reference_id | String |  | Bill Reference Id |
| 9 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 10 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 11 | data.channel_type | String | 4 | Channel Type |
| 12 | data.component_of_bill | String | 1 | Component Of Bill |
| 13 | data.contract_number | String | 12 | Contract Number |
| 14 | data.currency_code | String | 3 | Currency Code |
| 15 | data.customer_id | String | 12 | Customer Id |
| 16 | data.customer_name | String | 14 | Customer Name |
| 17 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 18 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 19 | data.date_time | String | 24 | Date Time |
| 20 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 21 | data.fwd_stan | String | 6 | Forwarding STAN |
| 22 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 23 | data.payment_mode | String | 2 | Payment Mode |
| 24 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 25 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 26 | data.receipt_bill_count | String | | Receipt Bill Count |
| 27 | data.receipt_customer_id | String | | Receipt Customer Id |
| 28 | data.receipt_customer_name | String | | Receipt Customer Name |
| 29 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 30 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 31 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 32 | data.receipt_meter_number | String | | Receipt Meter Number |
| 33 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 34 | data.receipt_penalty | String | | Receipt Penalty |
| 35 | data.receipt_period_id | String | | Receipt Period Id |
| 36 | data.receipt_product_name | String | | Receipt Product Name |
| 37 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 38 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 39 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 40 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 41 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 42 | data.receipt_tariff | String | | Receipt Tariff |
| 43 | data.reference_number | String | 11 | Reference Number |
| 44 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 45 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 46 | data.receipt | Array | | Receipt |

### Reversal Request

**Message Sample**
```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 516

{
    "command": "reversal",
    "product_code": "054501",
    "data": {
        "amount": 700000,
        "channel_type": 6024,
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "payment_mode": "12",
        "reference_number": "12345678902",
        "total_admin_fee": 2500
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | Integer | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 12 | M | Customer Id |
| 8 | data.customer_name | String | 14 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 11 | M | Reference Number |
| 14 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 15 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Reversal Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2478

{
    "biller_message": "",
    "biller_response": "",
    "command": "reversal",
    "product_code": "054501",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 700000,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "3",
        "contract_number": "541100064594",
        "currency_code": "IDR",
        "customer_id": "541100064594",
        "customer_name": "ACENG SURACENG",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320206141500",
        "fwd_stan": "863702",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "700.000",
        "receipt_bill_count": "3 BULAN",
        "receipt_customer_id": "541100064591",
        "receipt_customer_name": "ACENG SURACENG",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA (PLN)",
        "receipt_info_text_2": "MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": "BUKTI PEMBAYARAN YANG SAH",
        "receipt_meter_number": "",
        "receipt_paid_amount": "702.500",
        "receipt_penalty": "",
        "receipt_period_id": "JAN15,FEB15,MAR15",
        "receipt_product_name": "PLN POSTPAID",
        "receipt_reference_number_1": "0UAK24110000000086CA",
        "receipt_reference_number_2": "",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_stand_begin_end": "00400830-00631144",
        "receipt_tariff": "R3 \/000002200 VA",
        "reference_number": "12345678902",
        "total_admin_fee": 2500,
        "total_bill_amount": 700000,
        "receipt": [
            "BILLER : PLN POSTPAID",
            "IDPEL : 541100064591",
            "NAMA : ACENG SURACENG",
            "BL\/TH : JAN15,FEB15,MAR15",
            "STAND METER : 00400830-00631144",
            "",
            "TOTAL TGHN : 3 BULAN",
            "TARIF\/DAYA : R3 \/000002200 VA",
            "0UAK24110000000086CA",
            "",
            "RP TAG PLN : 700.000",
            "",
            "ADMIN BANK : 2.500",
            "TOTAL BAYAR : 702.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA (PLN)",
            "MENYATAKAN STRUK INI SEBAGAI",
            "BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.bill_reference_id | String |  | Bill Reference Id |
| 9 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 10 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 11 | data.channel_type | String | 4 | Channel Type |
| 12 | data.component_of_bill | String | 1 | Component Of Bill |
| 13 | data.contract_number | String | 12 | Contract Number |
| 14 | data.currency_code | String | 3 | Currency Code |
| 15 | data.customer_id | String | 12 | Customer Id |
| 16 | data.customer_name | String | 14 | Customer Name |
| 17 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 18 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 19 | data.date_time | String | 24 | Date Time |
| 20 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 21 | data.fwd_stan | String | 6 | Forwarding STAN |
| 22 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 23 | data.payment_mode | String | 2 | Payment Mode |
| 24 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 25 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 26 | data.receipt_bill_count | String | | Receipt Bill Count |
| 27 | data.receipt_customer_id | String | | Receipt Customer Id |
| 28 | data.receipt_customer_name | String | | Receipt Customer Name |
| 29 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 30 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 31 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 32 | data.receipt_meter_number | String | | Receipt Meter Number |
| 33 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 34 | data.receipt_penalty | String | | Receipt Penalty |
| 35 | data.receipt_period_id | String | | Receipt Period Id |
| 36 | data.receipt_product_name | String | | Receipt Product Name |
| 37 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 38 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 39 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 40 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 41 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 42 | data.receipt_tariff | String | | Receipt Tariff |
| 43 | data.reference_number | String | 11 | Reference Number |
| 44 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 45 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 46 | data.receipt | Array | | Receipt |

## Nonusage Electricity

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 312

{
    "command": "inquiry",
    "product_code": "053504",
    "data": {
        "channel_type": "6024",
        "contract_number": "1234567890123",
        "currency_code": "IDR",
        "date_time": "2019-08-08T23:56:59.987Z",
        "reference_number": "123456789491",
        "total_admin_fee": 2500
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.channel_type | String | 4 | M | Channel Type |
| 4 | data.contract_number | String | 13 | M | Contract Number |
| 5 | data.currency_code | String | 3 | M | Currency Code |
| 6 | data.date_time | String | 24 | M | Date Time |
| 7 | data.reference_number | String | 32 | M | Reference Number |
| 8 | data.total_admin_fee | Numeric | | M | Total Admin Fee |

### Inquiry Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 1852

{
    "biller_message": "",
    "biller_response": "",
    "command": "inquiry",
    "product_code": "053504",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 3000000,
        "balance_after": 1901581070,
        "balance_before": 1901581170,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "1234567890123",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320212180227",
        "fwd_stan": "787068",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "reference_number": "123456789491",
        "screen_admin_fee": "Rp 2.500",
        "screen_amount": "Rp 3.000.000",
        "screen_customer_id": "1234567890123",
        "screen_customer_name": "KUSUMO",
        "screen_header": "INFORMASI NON TAGIHAN LISTRIK",
        "screen_product_name": "PLN NONTAGLIS",
        "screen_reserved_1": "",
        "screen_total_amount": "Rp 3.002.500",
        "screen_transaction_type": "PERUBAHAN DAYA",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000,
        "screen": [
            "INFORMASI NON TAGIHAN LISTRIK",
            "",
            "BILLER : PLN NONTAGLIS",
            "NO REGISTRASI : 1234567890123",
            "TRANSAKSI : PERUBAHAN DAYA",
            "NAMA  : KUSUMO",
            "BIAYA PLN : Rp 3.000.000",
            "ADMIN BANK : Rp 2.500",
            "TOTAL BAYAR : Rp 3.002.500"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 13 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 13 | Customer Id |
| 18 | data.customer_name | String | 6 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.reference_number | String | 32 | Reference Number |
| 27 | data.screen_admin_fee | String | | Screen Admin Fee |
| 28 | data.screen_amount | String | | Screen Amount |
| 29 | data.screen_customer_id | String | | Screen Customer Id |
| 30 | data.screen_customer_name | String | | Screen Customer Name |
| 31 | data.screen_header | String | | Screen Header |
| 32 | data.screen_product_name | String | | Screen Product Name |
| 33 | data.screen_reserved_1 | String |  | Screen Reserved 1 |
| 34 | data.screen_total_amount | String | | Screen Total Amount |
| 35 | data.screen_transaction_type | String | | Screen Transaction Type |
| 36 | data.time_stamp | String | 24 | Time Stamp |
| 37 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 38 | data.total_bill_amount| Numeric | | Total Bill Amount |
| 39 | data.screen | Array |  | Screen |

### Payment Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 600

{
    "command": "payment",
    "product_code": "053504",
    "data": {
        "amount": 3000000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "payment_mode": "12",
        "reference_number": "123456789491",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 13 | M | Customer Id |
| 8 | data.customer_name | String | 6 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 32 | M | Reference Number |
| 14 | data.time_stamp | String | 24 | M | Time Stamp |
| 15 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 16 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Payment Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2601

{
    "biller_message": "",
    "biller_response": "",
    "command": "payment",
    "product_code": "053504",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 3000000,
        "balance_after": 1892575070,
        "balance_before": 1895577070,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "1234567890123",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "3.000.000",
        "receipt_customer_id": "532410477184",
        "receipt_customer_name": "KUSUMO",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA(PLN)",
        "receipt_info_text_2": " MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": " BUKTI PEMBAYARAN YANG SAH",
        "receipt_paid_amount": "3.002.500",
        "receipt_penalty": "",
        "receipt_product_name": "PLN NONTAGLIS",
        "receipt_reference_number_1": "0UAK2411000000008C42",
        "receipt_reference_number_2": "Rp 2.500",
        "receipt_registration_date": "25AUG10",
        "receipt_registration_number": "1234567890123",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_reserved_3": "",
        "receipt_stand_begin_end": "",
        "receipt_transaction_type": "PERUBAHAN DAYA4",
        "reference_number": "123456789491",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000,
        "receipt": [
            "BILLER : PLN NONTAGLIS",
            "NO REGISTRASI : 1234567890123",
            "NAMA : KUSUMO",
            "TGL REGISTRASI: 25AUG10",
            "",
            "TRANSAKSI : PERUBAHAN DAYA4",
            "IDPEL : 532410477184",
            "",
            "NO REFF : 0UAK2411000000008C42",
            "ADMIN : Rp 2.500",
            "BIAYA PLN : 3.000.000",
            "",
            "BIAYA ADMIN : 2.500",
            "TOTAL BAYAR : 3.002.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA(PLN)",
            " MENYATAKAN STRUK INI SEBAGAI",
            " BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 13 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 13 | Customer Id |
| 18 | data.customer_name | String | 6 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 27 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 28 | data.receipt_customer_id | String | | Receipt Customer Id |
| 29 | data.receipt_customer_name | String | | Receipt Customer Name |
| 30 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 31 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 32 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 33 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 34 | data.receipt_penalty | String | | Receipt Penalty |
| 35 | data.receipt_product_name | String | | Receipt Product Name |
| 36 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 37 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 38 | data.receipt_registration_date | String | | Receipt Registration Date |
| 39 | data.receipt_registration_number | String | | Receipt Registration Number |
| 40 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 41 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 42 | data.receipt_reserved_3 | String | | Receipt Reserved 3 |
| 43 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 44 | data.receipt_transaction_type | String | | Receipt Transaction Type |
| 45 | data.reference_number | String | 32 | Reference Number |
| 46 | data.time_stamp | String | 24 | Time Stamp |
| 47 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 48 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 49 | data.receipt | Array | | Receipt |


### Advice Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 599

{
    "command": "advice",
    "product_code": "053504",
    "data": {
        "amount": 3000000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "payment_mode": "12",
        "reference_number": "123456789491",
        "time_stamp": "2020-02-02T02:02:02.222Z,
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 13 | M | Customer Id |
| 8 | data.customer_name | String | 6 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 32 | M | Reference Number |
| 14 | data.time_stamp | String | 24 | M | Time Stamp |
| 15 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 16 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Advice Response

**Message Sample**
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2601

{
    "biller_message": "",
    "biller_response": "",
    "command": "payment",
    "product_code": "053504",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 3000000,
        "balance_after": 1892575070,
        "balance_before": 1895577070,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "1234567890123",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "3.000.000",
        "receipt_customer_id": "532410477184",
        "receipt_customer_name": "KUSUMO",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA(PLN)",
        "receipt_info_text_2": " MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": " BUKTI PEMBAYARAN YANG SAH",
        "receipt_paid_amount": "3.002.500",
        "receipt_penalty": "",
        "receipt_product_name": "PLN NONTAGLIS",
        "receipt_reference_number_1": "0UAK2411000000008C42",
        "receipt_reference_number_2": "Rp 2.500",
        "receipt_registration_date": "25AUG10",
        "receipt_registration_number": "1234567890123",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_reserved_3": "",
        "receipt_stand_begin_end": "",
        "receipt_transaction_type": "PERUBAHAN DAYA4",
        "reference_number": "123456789491",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000,
        "receipt": [
            "BILLER : PLN NONTAGLIS",
            "NO REGISTRASI : 1234567890123",
            "NAMA : KUSUMO",
            "TGL REGISTRASI: 25AUG10",
            "",
            "TRANSAKSI : PERUBAHAN DAYA4",
            "IDPEL : 532410477184",
            "",
            "NO REFF : 0UAK2411000000008C42",
            "ADMIN : Rp 2.500",
            "BIAYA PLN : 3.000.000",
            "",
            "BIAYA ADMIN : 2.500",
            "TOTAL BAYAR : 3.002.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA(PLN)",
            " MENYATAKAN STRUK INI SEBAGAI",
            " BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.balance_after | Integer | 10 | Balance After |
| 9 | data.balance_before | Integer | 10 | Balance Before |
| 10 | data.bill_reference_id | String |  | Bill Reference Id |
| 11 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 12 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 13 | data.channel_type | String | 4 | Channel Type |
| 14 | data.component_of_bill | String | 1 | Component Of Bill |
| 15 | data.contract_number | String | 13 | Contract Number |
| 16 | data.currency_code | String | 3 | Currency Code |
| 17 | data.customer_id | String | 13 | Customer Id |
| 18 | data.customer_name | String | 6 | Customer Name |
| 19 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 20 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 21 | data.date_time | String | 24 | Date Time |
| 22 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 23 | data.fwd_stan | String | 6 | Forwarding STAN |
| 24 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 25 | data.payment_mode | String | 2 | Payment Mode |
| 26 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 27 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 28 | data.receipt_customer_id | String | | Receipt Customer Id |
| 29 | data.receipt_customer_name | String | | Receipt Customer Name |
| 30 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 31 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 32 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 33 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 34 | data.receipt_penalty | String | | Receipt Penalty |
| 35 | data.receipt_product_name | String | | Receipt Product Name |
| 36 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 37 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 38 | data.receipt_registration_date | String | | Receipt Registration Date |
| 39 | data.receipt_registration_number | String | | Receipt Registration Number |
| 40 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 41 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 42 | data.receipt_reserved_3 | String | | Receipt Reserved 3 |
| 43 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 44 | data.receipt_transaction_type | String | | Receipt Transaction Type |
| 45 | data.reference_number | String | 32 | Reference Number |
| 46 | data.time_stamp | String | 24 | Time Stamp |
| 47 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 48 | data.total_bill_amount  | Numeric | | Total Bill Amount |
| 49 | data.receipt | Array | | Receipt |

### Reversal Request

**Message Sample**
```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-alto-key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-alto-timestamp: 2020-02-02T02:02:02.222Z
X-alto-signature: a01dcd3b41fd6e71ed55e4d92402826f7f369e0af01db194cc9d32d7eb2c322a
Content-length: 601

{
    "command": "reversal",
    "product_code": "053504",
    "data": {
        "amount": 3000000,
        "channel_type": "6024",
        "component_of_bill": "1",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "payment_mode": "12",
        "reference_number": "123456789491",
        "time_stamp": "2019-08-08T23:56:59.987Z",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000
    }
}
```

**Field Description**

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | String | | M | Product Code |
| 3 | data.amount | Numeric | | M | Amount |
| 4 | data.channel_type | String | 4 | M | Channel Type |
| 5 | data.component_of_bill | String | 1 | M | Component Of Bill |
| 6 | data.currency_code | String | 3 | M | Currency Code |
| 7 | data.customer_id | String | 13 | M | Customer Id |
| 8 | data.customer_name | String | 6 | M | Customer Name |
| 9 | data.date_time | String | 24 | M | Date Time |
| 10 | data.fwd_reference_number | String | 12 | M | Fwd Reference Number |
| 11 | data.fwd_stan | String | 6 | M | Forwarding STAN |
| 12 | data.payment_mode | String | 2 | M | Payment Mode |
| 13 | data.reference_number | String | 32 | M | Reference Number |
| 14 | data.time_stamp | String | 24 | M | Time Stamp |
| 15 | data.total_admin_fee | Numeric | | M | Total Admin Fee |
| 16 | data.total_bill_amount | Numeric | | M | Total Bill Amount |

### Reversal Response

**Sample Message**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Sun, 2 Feb 2020 02:02:02 GMT
Content-length: 2477

{
    "biller_message": "",
    "biller_response": "",
    "command": "reversal",
    "product_code": "053504",
    "response_code": "001",
    "response_text": "Success",
    "data": {
        "amount": 3000000,
        "bill_reference_id": "",
        "biller_aggregator_id": "654321981",
        "biller_aggregator_switch_id": "987654321",
        "channel_type": "6024",
        "component_of_bill": "1",
        "contract_number": "1234567890123",
        "currency_code": "IDR",
        "customer_id": "1234567890123",
        "customer_name": "KUSUMO",
        "customer_reference_id_1": "",
        "customer_reference_id_2": "",
        "date_time": "2019-08-08T23:56:59.987Z",
        "fwd_reference_number": "320211173156",
        "fwd_stan": "936719",
        "issuer_switch_id": "360003",
        "payment_mode": "12",
        "receipt_admin_fee": "2.500",
        "receipt_bill_amount": "3.000.000",
        "receipt_customer_id": "532410477184",
        "receipt_customer_name": "KUSUMO",
        "receipt_info_text_1": "PERUSAHAAN LISTRIK NEGARA(PLN)",
        "receipt_info_text_2": " MENYATAKAN STRUK INI SEBAGAI",
        "receipt_info_text_3": " BUKTI PEMBAYARAN YANG SAH",
        "receipt_paid_amount": "3.002.500",
        "receipt_penalty": "",
        "receipt_product_name": "PLN NONTAGLIS",
        "receipt_reference_number_1": "0UAK2411000000008C42",
        "receipt_reference_number_2": "Rp 2.500",
        "receipt_registration_date": "25AUG10",
        "receipt_registration_number": "1234567890123",
        "receipt_reserved_1": "",
        "receipt_reserved_2": "",
        "receipt_reserved_3": "",
        "receipt_stand_begin_end": "",
        "receipt_transaction_type": "PERUBAHAN DAYA4",
        "reference_number": "123456789491",
        "total_admin_fee": 2500,
        "total_bill_amount": 3000000,
        "receipt": [
            "BILLER : PLN NONTAGLIS",
            "NO REGISTRASI : 1234567890123",
            "NAMA : KUSUMO",
            "TGL REGISTRASI: 25AUG10",
            "",
            "TRANSAKSI : PERUBAHAN DAYA4",
            "IDPEL : 532410477184",
            "",
            "NO REFF : 0UAK2411000000008C42",
            "ADMIN : Rp 2.500",
            "BIAYA PLN : 3.000.000",
            "",
            "BIAYA ADMIN : 2.500",
            "TOTAL BAYAR : 3.002.500",
            "",
            "",
            "PERUSAHAAN LISTRIK NEGARA(PLN)",
            " MENYATAKAN STRUK INI SEBAGAI",
            " BUKTI PEMBAYARAN YANG SAH"
        ]
    }
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | biller_message | String |  | Biller Message |
| 2 | biller_response | String |  | Biller Response |
| 3 | command | String | | Transaction command |
| 4 | product_code | String | | Product Code |
| 5 | response_code | String | 3 | Response Code |
| 6 | response_text | String | | Response Text |
| 7 | data.amount | Numeric | | Amount |
| 8 | data.bill_reference_id | String |  | Bill Reference Id |
| 9 | data.biller_aggregator_id | String | 9 | Biller Aggregator Id |
| 10 | data.biller_aggregator_switch_id | String | 9 | Biller Aggregator Switch Id |
| 11 | data.channel_type | String | 4 | Channel Type |
| 12 | data.component_of_bill | String | 1 | Component Of Bill |
| 13 | data.contract_number | String | 13 | Contract Number |
| 14 | data.currency_code | String | 3 | Currency Code |
| 15 | data.customer_id | String | 13 | Customer Id |
| 16 | data.customer_name | String | 6 | Customer Name |
| 17 | data.customer_reference_id_1 | String | 1 | Customer Reference ID 1 (0 if customer_id is represent PLN Meter ID, 1 if customer_id represent PLN Customer Number) |
| 18 | data.customer_reference_id_2 | String |  | Customer Reference Id 2 |
| 19 | data.date_time | String | 24 | Date Time |
| 20 | data.fwd_reference_number | String | 12 | Fwd Reference Number |
| 21 | data.fwd_stan | String | 6 | Forwarding STAN |
| 22 | data.issuer_switch_id | String | 6 | Issuer Switch Id |
| 23 | data.payment_mode | String | 2 | Payment Mode |
| 24 | data.receipt_admin_fee | String | | Receipt Admin Fee |
| 25 | data.receipt_bill_amount | String | | Receipt Bill Amount |
| 26 | data.receipt_customer_id | String | | Receipt Customer Id |
| 27 | data.receipt_customer_name | String | | Receipt Customer Name |
| 28 | data.receipt_info_text_1 | String | | Receipt Info Text 1 |
| 29 | data.receipt_info_text_2 | String | | Receipt Info Text 2 |
| 30 | data.receipt_info_text_3 | String | | Receipt Info Text 3 |
| 31 | data.receipt_paid_amount | String | | Receipt Paid Amount |
| 32 | data.receipt_penalty | String | | Receipt Penalty |
| 33 | data.receipt_product_name | String | | Receipt Product Name |
| 34 | data.receipt_reference_number_1 | String | | Receipt Reference Number 1 |
| 35 | data.receipt_reference_number_2 | String | | Receipt Reference Number 2 |
| 36 | data.receipt_registration_date | String | | Receipt Registration Date |
| 37 | data.receipt_registration_number | String | | Receipt Registration Number |
| 38 | data.receipt_reserved_1 | String | | Receipt Reserved 1 |
| 39 | data.receipt_reserved_2 | String | | Receipt Reserved 2 |
| 40 | data.receipt_reserved_3 | String | | Receipt Reserved 3 |
| 41 | data.receipt_stand_begin_end | String | | Receipt Stand Begin End |
| 42 | data.receipt_transaction_type | String | | Receipt Transaction Type |
| 43 | data.reference_number | String | 32 | Reference Number |
| 44 | data.total_admin_fee | Numeric | | Total Admin Fee |
| 45 | data.total_bill_amount | Numeric | | Total Bill Amount |
| 46 | data.receipt | Array | | Receipt |


## Water

Drinking Water Company is a company domiciled in each region. Each company has a different policy that causes the message format is also different. Each water company will form a separate product.

If the customer does not enter mandatory data in both the inquiry and payment process, the biller might assume the transaction is invalid and will cause the transaction to fail. For this reason, every mandatory parameter must be filled in as appropriate.

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 526709aaf22e9523cb5dcea826e586b36955a17d6ceb0da06d9d629913d29c48
Content-length: 177

{
	"command": "inquiry",
	"product_code": "030010",
	"data":{
		"date_time": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
	        "channel_type": "6024",
	        "component_of_bill": "1",
	        "currency_code": "IDR",
		"customer_id": "781234567"
	}
}
```

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | M | Product code |
| 3 | `data`.date_time | String | 23 | M | Response time stamp |
| 4 | `data`.customer_id | String | 12 | M | Customer ID |
| 5 | `data`.channel_type | String | 4 | M | Channel type |
| 6 | `data`.currency_code | String | 3 | M | Currency code |

### Inquiry Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 363

{
	"response_code": "001",
	"biller_message": "Sukses",
	"data": {
		"amount": "235000",
		"time_stamp": "2019-08-28T09:44:18.650Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "781234567",
		"reference_number": "1566985456"
	},
	"biller_response": "Sukses",
	"response_text": "Success",
	"product_code": "030010",
	"command": "inquiry"
}

```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String | | Transaction command |
| 2 | product_type | String | | Product type |
| 3 | product_code | Numeric, can begin with 0 | | Product code |
| 4 | response_code | String | 3 | Response code |
| 5 | response_text | String | | Response text |
| 6 | `data`.date_time | String | 23 | Response time stamp |
| 7 | `data`.reference_number | String | 20 | Reference number |
| 8 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 9 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 10 | `data`.customer_id | String | 12 | Customer ID |
| 11 | `data`.customer_name | String | 40 | Customer Name |
| 12 | `data`.amount | Decimal | 12 | Amount |

### Payment Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 8a3f728a8b2ae70e80b46742c74dc9bad45dea8a9671bb7b2e403c6ba4cca125
Content-length: 296

{
	"command": "payment",
	"product_type": "030010",
	"data": {
		"locket_address": "Jalan Anggrek Neli Murni",
		"currency_code": "IDR",
		"locket_name": "ALTO",
		"merchant_type": "6021",
		"locket_code": "123",
		"reference_number": "1566985456",
		"locket_phone": "02199999",
		"date_time": "2019-08-28T16:46:08.000Z",
		"amount": "235000",
		"time_stamp": "2019-08-28T09:44:18.650Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "781234567"
	}
}
```

| No | Parameter | Type | Length | M/O/C | Description |
| -- | -- | -- | -- | -- | -- |
| 1 | command | String | | M | Transaction command |
| 2 | product_type | String | | M | Product type |
| 3 | product_code | Numeric, can begin with 0 | | M | Product code |
| 4 | `data`.date_time | String | 23 | M | Response time stamp |
| 5 | `data`.reference_number | String | 20 | M | Reference number |
| 6 | `data`.fwd_reference_number | String | 32 | M | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 7 | `data`.fwd_stan | Numeric | 6 | | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 8 | `data`.customer_id | String | 12 | M | Customer ID |
| 9 | `data`.customer_name | String | 40 | M | Customer Name |
| 10 | `data`.amount | Decimal | 12 | M | Amount |
| 11 | `data`.merchant_type | Numeric | 4 | M | Merchant type of the channel where customer pay the bill |
| 12 | `data`.locket_code | String | 32 | O | Locket code where customer pay |
| 13 | `data`.locket_name | String | 30 | O | Locket name where customer pay |
| 14 | `data`.locket_address | String | 50 | O | Locket address where customer pay |
| 15 | `data`.locket_phone | String | 18 | O | Locket phone where customer pay |

### Payment Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 363

{
	"response_code": "001",
	"biller_message": "Sukses",
	"data": {
		"amount": "235000",
		"time_stamp": "2019-08-28T09:46:09.458Z",
		"fwd_stan": "000025",
		"customer_name": "Kamshory",
		"fwd_reference_number": "00000000869",
		"customer_id": "781234567",
		"reference_number": "1566985456"
	},
	"biller_response": "Sukses",
	"response_text": "Success",
	"product_code": "030010",
	"command": "payment"
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String |  | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | Product code |
| 3 | response_code | String | | Response code |
| 4 | response_text | String | | Response text |
| 5 | `data`.date_time | String | 23 | Response time stamp |
| 6 | `data`.reference_number | String | 32 | Reference number |
| 7 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 8 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 9 | `data`.customer_id | String | 12 | Customer ID |
| 10 | `data`.customer_name | String | 40 | Customer Name |
| 11 | `data`.amount | Decimal | 12 | Amount |

## General Bill

### Inquiry Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 526709aaf22e9523cb5dcea826e586b36955a17d6ceb0da06d9d629913d29c48
Content-length: 177

{
	"command": "inquiry",
	"product_code": "482382366",
	"data":{
		"date_time": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"customer_id": "081234567"
	}
}
```

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String |  | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | Product code |
| 3 | `data`.date_time | String | 23 | Response time stamp |
| 4 | `data`.reference_number | String | 32 | Reference number |
| 5 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 6 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 7 | `data`.customer_id | String | 12 | Customer ID |

### Inquiry Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 364

{
	"command": "payment",
	"product_code": "482382366",
	"response_code": "001",
	"response_text": "Success",
	"command": "payment",
	"data":{
		"time_stamp": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"fwd_reference_number": "99999",
		"fwd_stan": "556287",
		"customer_id": "08123456789",
		"customer_name": "Jono",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String |  | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | Product code |
| 3 | response_code | String | | Response code |
| 4 | response_text | String | | Response text |
| 5 | `data`.date_time | String | 23 | Response time stamp |
| 6 | `data`.reference_number | String | 32 | Reference number |
| 7 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 8 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 9 | `data`.customer_id | String | 12 | Customer ID |
| 10 | `data`.customer_name | String | 40 | Customer Name |
| 11 | `data`.amount | Decimal | 12 | Amount |

### Payment Request

**Message Sample**

```http
POST /v2/bill-payment/process-biller HTTP/1.1
Content-type: application/json
Accept: application/json
Content-encoding: identity
Accept-encoding: identity
Host: api-sandbox.altopay.id
Connection: close
User-agent: Planet POS
X-Alto-Key: 650a8e7e-b97f-11e9-a2a3-2a2ae2dbcce4
X-Alto-Timestamp: 2019-08-08T08:08:08.789Z
X-Alto-Signature: 41479a908c083b4c782b2fe8be7f9582c932d83126e5ad3fa3fde941b025a6c1
Content-length: 285

{
	"command": "payment",
	"product_code": "482382366",
	"data":{
		"date_time": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"fwd_reference_number": "99999",
		"fwd_stan": "556287",
		"customer_id": "081234567",
		"customer_name": "Jono",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String |  | Transaction command |
| 2 | product_code | Numeric, can begin with 0 |  | Product code |
| 3 | `data`.date_time | String | 23 | Response time stamp |
| 4 | `data`.reference_number | String | 32 | Reference number |
| 5 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 6 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 7 | `data`.customer_id | String | 12 | Customer ID |
| 8 | `data`.customer_name | String | 40 | Customer Name |
| 9 | `data`.amount | Decimal | 12 | Amount |
| 10 | `data`.merchant_type | Numeric | 4 | Merchant type of the channel where customer pay the bill |
| 11 | `data`.locket_code | String | 32 | Locket code where customer pay |
| 12 | `data`.locket_name | String | 30 | Locket name where customer pay |
| 13 | `data`.locket_address | String | 50 | Locket address where customer pay |
| 14 | `data`.locket_phone | String | 18 | Locket phone where customer pay |

### Payment Response

**Message Sample**

```http
HTTP/1.1 200 OK
Content-type: application/json
Content-encoding: identity
Date: Thu, 8 Aug 2019 23:56:59 GMT
Content-length: 364

{
	"command": "payment",
	"product_code": "482382366",
	"response_code": "001",
	"response_text": "Success",
	"command": "payment",
	"data":{
		"time_stamp": "2019-08-08T23:56:59.987Z",
		"reference_number": "1234567889",
		"fwd_reference_number": "99999",
		"fwd_stan": "556287",
		"customer_id": "08123456789",
		"customer_name": "Jono",
		"amount": "100000"
	}
}
```

**Field Description**

| No | Parameter | Type | Length | Description |
| -- | -- | -- | -- | -- |
| 1 | command | String | | Transaction command |
| 2 | product_code | Numeric, can begin with 0 | | Product code |
| 3 | response_code | String | | Response code |
| 4 | response_text | String | | Response text |
| 5 | `data`.time_stamp | String | 23 | Response time stamp |
| 6 | `data`.reference_number | String | 32 | Reference number |
| 7 | `data`.fwd_reference_number | String | 32 | For several product, fwd_reference_number must be same with fwd_reference_number on inquiry response |
| 8 | `data`.fwd_stan | Numeric | 6 | For several product, fwd_stan must be same with fwd_stan on inquiry response |
| 9 | `data`.customer_id | String | 12 | Customer ID |
| 10 | `data`.customer_name | String | 40 | Customer Name |
| 11 | `data`.amount | Decimal | 12 | Amount |