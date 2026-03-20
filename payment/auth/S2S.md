# Service-to-service security

Сервисы вызывают друг друга с проверкой mTLS для задача по cron, без участия пользователя  
Каждому сервисы выдаётся свой собственный сертификат для идентификации сервиса. Еще до передачи данных сервисы сверяют свертефикаты и принимает, либо же отклоняет запрос.  
Для вызовов пользователя используется token propagation, то есть мы вместе с запросом в заголовках передаем JWT токен для последующих AuthZ  

Callback → Transaction - token propagation  
Orchestrator → Transaction - token propagation  
Orchestrator → Wallet - token propagation  
Orchestrator → Transaction Query - token propagation  
Wallet → User - token propagation  
Antifraud → Wallet - mTLS  

## Таблица доверия
| Кто кого вызывает | Каким токеном | Где проверяется | Scopes / Roles |
|---|---|---|---|
| Callback → Transaction|JWT access token | Transaction  |EXT_SYSTEM  |
| Orchestrator → Transaction|JWT access token |Transaction |USER,ADMIN|
| Orchestrator → Wallet|JWT access token |Wallet |USER,ADMIN |
| Orchestrator → Transaction Query|JWT access token |Transaction Query |USER,ADMIN |
| Wallet → User|JWT access token |User |USER,ADMIN |
| Antifraud → Wallet|mTLS |на стороне Wallet при запросе |Без роли|

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
