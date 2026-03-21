# Плагины и проверки на уровне API Gateway

Для работы с Idp Keycloak и сессиями в Redis необходим плагин traefikoidc    
Проверка HMAC встреона в Traefik - плагины не понадобятся
Для проверки реплаев по eventId - пишем кастомный плагин на Go

На уровне API Gateway существуют проверки:
- rate limitting
- проверка HMAC для платежного провайдера
- Интеграция с Keyloack и проверка сессии в redis
- проверка реплаев по eventId в redis