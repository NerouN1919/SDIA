# Формат событий и структура полей

Формат - JSON  
Kafka топики: payment.initiated(PaymentInitiated), payment.result(PaymentResult), payment.wallet.result(PaymentCompleted, PaymentFailed)  
Rabbit топики: notification.sent(NotificationSent)  
Порядок в Kafka гарантируется с помощью ключа wallet_id  
В хэдерах - тип эвента  

## payment.initiated
Идемпотентность - wallet_transactions.id
```json
{
  "payment_id": "uuid",
  "walletTransactionId": "uuid",
  "walletId": "uuid",
  "userId": "uuid",
  "amount": 0.00
}
```

## payment.result
Идемпотентность - paymentId
```json
{
  "walletTransactionId": "uuid",
  "paymentId": "uuid",
  "externalPaymentId": "",
  "status": "SUCCESS",
  "failureReason": null
}
```

## payment.wallet.result
Идемпотентность - walletTransactionId
```json
{
  "walletTransactionId": "uuid",
  "walletId": "uuid",
  "userId": "uuid",
  "paymentId": "uuid",
  "amount": 200.00,
  "finalStatus": "COMPLETED",
  "failureReason": ""
}
```

## notification.sent

```json
    {
  "userId": "uuid",
  "walletTransactionId": "uuid",
  "amount": 200.00,
  "finalStatus": "COMPLETED",
  "message": "Платёж на сумму 200.00 RUB успешно выполнен"
}
```