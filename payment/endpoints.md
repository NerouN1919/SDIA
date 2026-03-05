# API эндпоинты

## Orchestrator

### POST /api/v1/payments
Оформить платёж пользователю
Body:  
```json
{
    "cardNumber": string,
    "amount": int
}
```  
Response - 202 Accepted
Response Body:  
```json
{
    "payment_id": uuid,
    "sratus": string
}
```

### GET /api/v1/wallet
Внутренний вызов Wallet Service

### GET /api/v1/payments?{filters}
Получить все платежи пользователя(возможно с фильтрами)  
filters - фильтры для выборки(опционально)

### GET /api/v1/payments/{id}
Получить платёж по id  
id - id платежа в transactions

### POST /api/v1/wallet/funds/reserve
Внутренний вызов Wallet Service
Body:  
```json
{
    "user_id": uuid,
    "amount": int
}
```  
Response - 200 OK

### POST /api/v1/wallet/funds/commit
Внутренний вызов Wallet Service
Body:  
```json
{
    "user_id": uuid,
    "transaction_id": uuid
}
```  
Response - 200 OK

### POST /api/v1/wallet/funds/release
Внутренний вызов Wallet Service
Body:  
```json
{
    "user_id": uuid,
    "transaction_id": uuid
}
```  
Response - 200 OK

### POST /api/v1/transaction/callback
Внутренний вызов Transactions Service
Body:  
```json
{
    "payment_id": uuid,
    "status": string
}
```  
Response - 200 OK

## Wallet

### POST /api/v1/payments
Оформить платёж пользователю
Body:  
```json
{
    "cardNumber": string,
    "amount": int
}
```  
Response - 202 Accepted
Response Body:  
```json
{
    "payment_id": uuid,
    "sratus": string
}
```

### POST /api/v1/wallet/funds/reserve
Зарезервировать деньги на счете клиента  
Body:  
```json
{
    "user_id": uuid,
    "amount": int
}
```  
Response - 200 OK

### POST /api/v1/wallet/funds/commit
Перевести резервирование в списание
Body:  
```json
{
    "user_id": uuid,
    "transaction_id": uuid
}
```  
Response - 200 OK

### POST /api/v1/wallet/funds/release
Отменить резервирование денег
Body:  
```json
{
    "user_id": uuid,
    "transaction_id": uuid
}
```  
Response - 200 OK

### GET /api/v1/wallet
Получить информацию о кошельке для теущего пользователя

## Transactions

### POST /api/v1/transaction/callback
Обработка Callback
Body:  
```json
{
    "payment_id": uuid,
    "status": string
}
```  
Response - 200 OK

## Provider

### POST /api/v1/payment

Body:  
```json
{
    "callback_url": string,
    "transaction_id": uuid,
    "amount": srting
}
```  
Response - 200 OK  
Response Body:  
```json
{
    "payment_id": uuid
}
```