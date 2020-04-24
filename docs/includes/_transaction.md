# Transaction Flow
![](transaction_flow.png)  

When customer pays bills through a bank Virtual Account, customer makes an inquiry of the Virtual Account number. Bank will send an inquiry request to ALTOPay and will be forwarded to merchant.  

Merchants must provide an API that will be accessed by ALTOPay when making an inquiry and payment.  

Merchant must response the inquiry request from ALTOPay in accordance with the specified message specifications. This response will be forwarded to bank to be displayed to customer.  

When customer pays a bill, bank will send a payment request to ALTOPay and will be forwarded to merchant. Merchant must ensure that the payment number and payment amount are in accordance with the bill or not. If it’s in accordance with the bill, merchant must indicate that the bill has been paid and send response to ALTOPay that the payment has been successful. Otherwise, if the payment number and payment amount does not match the amount of the bill, then merchant must reject the payment.  