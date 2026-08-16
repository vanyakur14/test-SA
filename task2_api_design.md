# Задание 2: Проектирование API (Экран выбора магазинов)

## 1. Общее описание

Для нового экрана мобильного приложения «Петрушка Зеленая», на котором отображаются магазины-партнёры с возможностью перехода на внешний ресурс, спроектирован REST API эндпоинт.

Основные требования к API:
- Получение списка магазинов на основе **геолокации** пользователя (широта и долгота).
- Сортировка магазинов по релевантности (приоритет партнёрства / расстояние / время доставки).
- Каждый магазин содержит **ссылку (url)** для перехода по клику на плашку.
- Поддержка пагинации для длинных списков.

---

## 2. Спецификация запроса

### 2.1. Эндпоинт

| Метод | URL |
| :--- | :--- |
| **GET** | `/api/v1/stores/nearby` |

### 2.2. Заголовки (Headers)

| Заголовок | Значение | Обязательность | Описание |
| :--- | :--- | :--- | :--- |
| `Authorization` | `Bearer <access_token>` | Да | Токен авторизации пользователя |
| `Accept` | `application/json` | Да | Формат ответа |
| `Accept-Language` | `ru-RU` | Нет | Язык локализации (по умолчанию русский) |

### 2.3. Параметры запроса (Query Params)

| Параметр | Тип | Обязательный | Описание | Пример |
| :--- | :--- | :--- | :--- | :--- |
| `lat` | `number` (decimal) | **Да** | Широта текущего местоположения пользователя | `55.7558` |
| `lng` | `number` (decimal) | **Да** | Долгота текущего местоположения пользователя | `37.6173` |
| `limit` | `integer` | Нет | Максимальное количество магазинов в ответе (по умолчанию — `10`, максимум — `50`) | `5` |
| `offset` | `integer` | Нет | Количество записей для пропуска (пагинация). По умолчанию — `0` | `10` |

### 2.4. Пример полного URL

```text
https://api.petrushka-green.ru/api/v1/stores/nearby?lat=55.7558&lng=37.6173&limit=4&offset=0

### 3. Пример успешного ответа

{
  "data": [
    {
      "id": "metro_001",
      "name": "METRO",
      "logo": "https://cdn.petrushka-green.ru/logos/metro.png",
      "deliveryTime": "Ближайшая доставка сегодня 21:00–23:00",
      "url": "https://metro.ru/special-offer?utm_source=petrushka",
      "rating": 4.8,
      "distance": 1.2
    },
    {
      "id": "ashan_002",
      "name": "Ашан",
      "logo": "https://cdn.petrushka-green.ru/logos/ashan.png",
      "deliveryTime": "Ближайшая доставка сегодня 18:00–20:00",
      "url": "https://ashan.ru/promo?ref=petrushka",
      "rating": 4.5,
      "distance": 2.7
    },
    {
      "id": "vkusvill_003",
      "name": "ВкусВилл",
      "logo": "https://cdn.petrushka-green.ru/logos/vkusvill.png",
      "deliveryTime": "Быстрая доставка от 20 до 60 минут",
      "url": "https://vkusvill.ru/express?partner=petrushka",
      "rating": 4.9,
      "distance": 0.8
    },
    {
      "id": "victoria_004",
      "name": "ВИКТОРИЯ",
      "logo": "https://cdn.petrushka-green.ru/logos/victoria.png",
      "deliveryTime": "Ближайшая доставка сегодня 17:00–19:00",
      "url": "https://victoria.ru/current-deals",
      "rating": 4.2,
      "distance": 3.1
    }
  ],
  "meta": {
    "total": 4,
    "limit": 4,
    "offset": 0
  }
}
