# Поток / сценарий

| Поток / сценарий | Брокер | Тип (event/command/job) | Есть DLQ? | Обработка DLQ / комментарий |
|---|---|---|---|---|
| PaymentInitiated -> Transaction Service | Kafka | event | Да | При неудачной попытке распарсить ообщение - оно попадает в DLQ(payment.initiated.dlq) и алертится разработчикам |
| PaymentResult -> Wallet Service | Kafka | event | Да | Если не удаётся вернуть средства пользователю, то сообщение кладется в DLQ(payment.result.dlq). |
| PaymentCompleted -> Transaction Query Service | Kafka | event | Да | Без DLQ(payment.wallet.result.dlq) read модель не будет поддерживать eventual consistency(возможная ошибка парсинга или недоступность API ElsaticSearch) |
| PaymentFailed Transaction Query Service -> | Kafka | event | Да | Без DLQ(payment.wallet.result.dlq) read модель не будет поддерживать eventual consitnency(возможная ошибка парсинга или недоступность API ElsaticSearch). Если сообщение не обработалось 3 раза, то оно записывается в DLQ и коммитится оффсет. Так топик не повиснет из-за невалдиных сообщений |
| NotificationSent -> Notification Service | RabbitMQ | event | Нет | Уведомления не критичны, пользователь видит и так актуальные данные в Read модели |  

В основном все сообщения идут через основные топики:
- Kafka - payment.initiated(PaymentInitiated), payment.result(PaymentResult), payment.wallet.result(PaymentCompleted, PaymentFailed) 
- RabbitMQ - notification.sent(NotificationSent)  
Ошибки обработки - идут в DLQ топики, с припиской ```.dlq```

При большой нагрузке в Kafka поддерживает backpressure через buffer.memory и acks. При увеличении нагрузки - требуется горизонтально масштабировать консьюмеров и увеличивать число партиций топика. Консьюмеров должно быть не большей партиций, иначе лишние будут простаивать
В RabbitMQ есть механизм backpressure, тем самым мы можем контролировать кол-во поступаемых сообщений, а те что не можем - блокируем продьюсеров.  