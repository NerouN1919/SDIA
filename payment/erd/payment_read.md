```
Table payment_projection {
  payment_id uuid [pk]
  wallet_transaction_id uuid [not null]
  wallet_id uuid [not null]
  user_id uuid [not null]
  amount DECIMAL(19,2) [not null]
  status varchar [not null, note: 'INITIATED, COMPLETED, FAILED']
  provider_payment_id varchar
  created_at timestamp [not null]
  updated_at timestamp [not null]

  Note: 'Read-модель в ElasticSearch'
}
```