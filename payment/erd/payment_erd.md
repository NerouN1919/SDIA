```
Table payments {
  id uuid [pk]
  provider_payment_id uuid
  wallet_transaction_id uuid [not null]
  amount DECIMAL(19,2) [not null]
  provider_fee_amount DECIMAL(19,2)
  status varchar [not null, note: 'PENDING, PROVIDER_PROCESSED, COMPLETED, FAILED']
  created_at timestamp [not null]
  updated_at timestamp [not null]
}


Table payment_outbox {
  id uuid [pk]
  payment_id uuid [not null]
  payload jsonb [not null]
  event_type varchar [not null, note: 'PaymentResult']
  created_at timestamp [not null]
  trace_id uuid
}

Ref: payment_outbox.payment_id > payments.id
```