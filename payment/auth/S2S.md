# Service-to-service security

Сервисы вызывают друг друга с проверкой mTLS
Каждому сервисы выдаётся свой собственный сертификат для идентификации сервиса. Еще до передачи данных сервисы сверяют свертефикаты и принимает, либо же отклоняет запрос.  
Для вызовов пользователя используется service JWT, то есть мы вместе с запросом в заголовках передаем JWT токен(внутренний токен для межсервисного взаимодейтсвия) для последующих AuthZ  

Callback → Transaction - service JWT  
Orchestrator → Transaction - service JWT  
Orchestrator → Wallet - service JWT  
Orchestrator → Transaction Query - service JWT  
Wallet → User - service JWT  
Antifraud → Wallet - mTLS  

## Таблица доверия
| Кто кого вызывает | Каким токеном | Где проверяется | Scopes / Roles |
|---|---|---|---|
| Callback → Transaction|Service JWT| Transaction  |EXT_SYSTEM  |
| Orchestrator → Transaction|Service JWT |Transaction |SERVICE|
| Orchestrator → Wallet|Service JWT |Wallet |SERVICE |
| Orchestrator → Transaction Query|Service JWT |Transaction Query |SERVICE |
| Wallet → User|Service JWT|User |SERVICE |
| Antifraud → Wallet|mTLS |на стороне Wallet при запросе |SERVICE|

# 12 factor

В окружении храним все переменные, которые на статические:
- эндпоинты
- URL сервисов
- URL БД
- конфиги сервисов
В засекреченном виде:
- Пароли от БД
- client_id и client_secret для Keycloak
- приватные ключи mTLS
- provider API key
- webhook signing secret

Нельзя хардкодить возможные динамически изменяемые данные, так как в этом случае придется рестартить сервис для их изменений, что создаст downtime  
Конфиги версионируются с помощью репозитория ARGO, где мы под каждые изменения создаем коммит и можем их переопределять  
Для различных сред мы создает разные конфиг файлы, которые в зависимости от среды будут подтягиваться
