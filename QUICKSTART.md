# Быстрый старт

Демонстрация Сценария 3: Прямой вход через Zitadel.

## Запуск

```bash
# Установка зависимостей
yarn install

# Запуск dev server
yarn dev
```

Приложение откроется на `http://localhost:5173`

## Использование

1. Откройте `http://localhost:5173`
2. Кликните **Login** в навигации
3. Введите credentials пользователя Zitadel
4. После аутентификации увидите JWT claims (prid, scp, x_user_name)

## Конфигурация

См. файл `.env`:
- `VITE_ZITADEL_PROJECT_RESOURCE_ID` — Project ID из Zitadel
- `VITE_ZITADEL_CLIENT_ID` — Application Client ID
- `VITE_ZITADEL_ISSUER` — Zitadel issuer URL

## Документация

Полная документация: [../README.md](../README.md)
