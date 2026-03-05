```
Table wallet {
  id uuid [pk]
  user_id uuid [not null]
  balance DECIMAL(19,2) [not null]
  reserved DECIMAL(19,2) [not null, default: 0]
  updated_at timestamp [not null]
}

Table wallet_transactions {
  id uuid [pk]
  wallet_id uuid [not null]
  status varchar [not null, note: 'PENDING, COMPLETED, FAILED']
  amount DECIMAL(19,2) [not null]
  created_at timestamp [not null]
  updated_at timestamp [not null]
}

Table wallet_outbox {
  id uuid [pk]
  transaction_id uuid [not null]
  event_type varchar [not null, note: 'PaymentInitiated, PaymentCompleted, PaymentFailed, NotificationSent']
  payload jsonb [not null]
}

Ref: wallet_transactions.wallet_id > wallet.id
Ref: wallet_outbox.transaction_id > wallet_transactions.id
```