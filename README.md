# Тестовое Vue приложение для Zitadel OIDC

Демонстрация **Сценария 3: Прямой вход через Zitadel** из [Архитектуры решения](https://wiki.yandex.ru/homepage/datadigital/it/architecture/arxitektura-reshenijj/korp.-portal/autentifikacija/#scenarij-3-pryamoj-vhod-cherez-zitadel).

## Описание

Приложение реализует OIDC Authorization Code Flow с PKCE для аутентификации пользователей через Zitadel и отображает JWT claims, специфичные для eFlow портала:

- `prid` — Project Resource ID
- `scp` — Scopes/роли пользователя
- `x_user_name` — Username пользователя

## Структура

```
zitadel-vue/
├── src/
│   ├── views/
│   │   └── LoginView.vue        # Страница с JWT claims (модифицирована)
│   ├── services/
│   │   └── zitadelAuth.ts       # Конфигурация @zitadel/vue
│   ├── App.vue                  # Главный компонент с навигацией
│   └── main.ts                  # Entry point
├── .env                         # Конфигурация Zitadel endpoints
└── package.json
```

## Конфигурация

### Переменные окружения (.env)

```env
VITE_ZITADEL_PROJECT_RESOURCE_ID=343323774046703118
VITE_ZITADEL_CLIENT_ID=345529978907069966
VITE_ZITADEL_ISSUER=https://zitadel.astrazenecacloud.ru
```

## Установка

```bash
cd zitadel-vue
yarn install
```

Или с npm:

```bash
npm install
```

## Запуск

### Development mode

```bash
yarn dev
```

Приложение будет доступно на: `http://localhost:5173`

### Type checking

```bash
yarn type-check
```

### Build для production

```bash
yarn build
```

### Preview production build

```bash
yarn preview
```

## Использование

1. Откройте `http://localhost:5173` в браузере
2. Нажмите **Login** в навигации
3. Будет выполнен redirect на Zitadel (`https://zitadel.astrazenecacloud.ru`)
4. Выберите **login** и введите credentials пользователя Zitadel
5. После успешной аутентификации будет redirect обратно в приложение
6. На странице `/login` отобразятся JWT claims:
   - **Ключевые claims для eFlow** (prid, scp, x_user_name)
   - Все остальные claims из userinfo endpoint

## OIDC Flow

Приложение реализует стандартный **Authorization Code Flow с PKCE**:

1. **Инициация:** `GET /oauth/v2/authorize` с параметрами:
   - `response_type=code`
   - `client_id=345529978907069966`
   - `redirect_uri=http://localhost:5173/auth/signinwin/zitadel`
   - `scope=openid profile email urn:zitadel:iam:org:project:id:343323774046703118:aud`

2. **Callback:** Zitadel возвращает `code` в query параметрах

3. **Token exchange:** `POST /oauth/v2/token` с `grant_type=authorization_code`

4. **Userinfo:** Обогащение профиля через `GET /oidc/v1/userinfo`
