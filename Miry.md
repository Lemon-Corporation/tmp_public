### Распределение эндпоинтов по важности

## **Эндпоинты высокого приоритета**

### 1. **POST `/auth/register`**  
**Описание**: Регистрация нового пользователя.  
**Что передает**:  
```json
{
  "email": "user@example.com",
  "username": "User123",
  "password": "secure_password"
}
```  
**Что возвращает**:  
- Успех (201 Created):  
  ```json
  {
    "user_id": 1,
    "email": "user@example.com",
    "username": "User123",
    "message": "User successfully registered."
  }
  ```  
- Ошибка (400 Bad Request):  
  ```json
  {
    "error": "Email already in use."
  }
  ```  
**Зачем нужен**: Базовый функционал, позволяющий пользователям создать аккаунт.

---

### 2. **POST `/auth/login`**  
**Описание**: Авторизация пользователя.  
**Что передает**:  
```json
{
  "email": "user@example.com",
  "password": "secure_password"
}
```  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5..."
  }
  ```  
- Ошибка (401 Unauthorized):  
  ```json
  {
    "error": "Invalid email or password."
  }
  ```  
**Зачем нужен**: Без авторизации пользователь не может получить доступ к своим мирам.

---

### 3. **POST `/worlds/create`**  
**Описание**: Создание нового мира.  
**Что передает**:  
```json
{
  "name": "My First World",
  "description": "A cool place to hang out."
}
```  
**Что возвращает**:  
- Успех (201 Created):  
  ```json
  {
    "world_id": 42,
    "name": "My First World",
    "description": "A cool place to hang out.",
    "created_at": "2024-11-19T12:34:56Z"
  }
  ```  
- Ошибка (400 Bad Request):  
  ```json
  {
    "error": "World name is required."
  }
  ```  
**Зачем нужен**: Позволяет пользователям начать работу с платформой, создавая свои первые миры.

---

### 4. **GET `/worlds/{world_id}`**  
**Описание**: Получение информации о конкретном мире.  
**Что передает**:  
- `world_id` в URL.  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "world_id": 42,
    "name": "My First World",
    "description": "A cool place to hang out.",
    "channels": [
      {
        "channel_id": 1,
        "name": "General",
        "type": "text"
      },
      {
        "channel_id": 2,
        "name": "Voice Chat",
        "type": "voice"
      }
    ]
  }
  ```  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "World not found."
  }
  ```  
**Зачем нужен**: Отображает детали мира, включая его каналы.

---

### 5. **POST `/channels/create`**  
**Описание**: Создание нового канала в указанном мире.  
**Что передает**:  
```json
{
  "world_id": 42,
  "name": "General Chat",
  "type": "text"
}
```  
**Что возвращает**:  
- Успех (201 Created):  
  ```json
  {
    "channel_id": 7,
    "name": "General Chat",
    "type": "text",
    "world_id": 42
  }
  ```  
- Ошибка (400 Bad Request):  
  ```json
  {
    "error": "Channel name is required."
  }
  ```  
**Зачем нужен**: Основной способ кастомизации мира — добавление текстовых или голосовых каналов.

---

### 6. **GET `/channels/{channel_id}`**  
**Описание**: Получение информации о конкретном канале.  
**Что передает**:  
- `channel_id` в URL.  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "channel_id": 7,
    "name": "General Chat",
    "type": "text",
    "messages": []
  }
  ```  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "Channel not found."
  }
  ```  
**Зачем нужен**: Показывает детали канала, включая тип и историю сообщений (для текстовых).

---

## **Эндпоинты среднего приоритета**

### 7. **POST `/messages/send`**  
**Описание**: Отправка сообщения в текстовый канал.  
**Что передает**:  
```json
{
  "channel_id": 7,
  "content": "Hello, world!"
}
```  
**Что возвращает**:  
- Успех (201 Created):  
  ```json
  {
    "message_id": 101,
    "channel_id": 7,
    "content": "Hello, world!",
    "timestamp": "2024-11-19T12:45:00Z"
  }
  ```  
- Ошибка (400 Bad Request):  
  ```json
  {
    "error": "Message content is required."
  }
  ```  
**Зачем нужен**: Позволяет пользователям общаться внутри текстовых каналов.

---

### 8. **DELETE `/worlds/{world_id}`**  
**Описание**: Удаление мира.  
**Что передает**:  
- `world_id` в URL.  
**Что возвращает**:  
- Успех (204 No Content).  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "World not found."
  }
  ```  
**Зачем нужен**: Удаляет мир и связанные с ним каналы.

---

### 9. **PATCH `/worlds/{world_id}/update`**  
**Описание**: Обновление информации о мире.  
**Что передает**:  
```json
{
  "name": "Updated World Name",
  "description": "Updated description."
}
```  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "world_id": 42,
    "name": "Updated World Name",
    "description": "Updated description."
  }
  ```  
**Зачем нужен**: Позволяет редактировать название и описание мира.

## Эндпоинт для подключения к голосовому каналу обычно реализуется с использованием WebSocket или другого протокола реального времени

---
### **POST /channels/{channel_id}/join-voice**

**Описание**: Подключение пользователя к голосовому каналу.

#### **Что передает**:
Параметры:
- `channel_id` (в URL): Идентификатор голосового канала.

Тело запроса:
```json
{
  "user_id": 1
}
```

#### **Что возвращает**:

**Успех (200 OK)**:
```json
{
  "message": "Successfully joined the voice channel.",
  "voice_server": "wss://voice.example.com",
  "channel_id": 15,
  "user_id": 1
}
```

**Ошибка (404 Not Found)**:
```json
{
  "error": "Voice channel not found."
}
```

**Ошибка (400 Bad Request)**:
```json
{
  "error": "User is already in a voice channel."
}
```

---

### **WebSocket /voice**

**Описание**: После получения URL голосового сервера клиент открывает WebSocket-соединение для передачи и получения аудио.

#### **Примеры сообщений через WebSocket**:

1. **Подключение к голосовому каналу**:
   Клиент отправляет:
   ```json
   {
     "action": "join",
     "user_id": 1,
     "channel_id": 15
   }
   ```

   Сервер отвечает:
   ```json
   {
     "status": "connected",
     "channel_id": 15,
     "user_id": 1,
     "participants": [
       {
         "user_id": 2,
         "username": "User456"
       },
       {
         "user_id": 3,
         "username": "User789"
       }
     ]
   }
   ```

2. **Передача аудио**:
   Клиент отправляет закодированные пакеты аудио (обычно в формате Opus).

3. **Отключение**:
   Клиент отправляет:
   ```json
   {
     "action": "leave",
     "user_id": 1,
     "channel_id": 15
   }
   ```

   Сервер подтверждает:
   ```json
   {
     "status": "disconnected",
     "channel_id": 15,
     "user_id": 1
   }
   ```

#### **Зачем нужен**:
Эндпоинт позволяет пользователям подключаться к голосовому каналу, после чего взаимодействие переходит на уровень WebSocket для передачи аудио в реальном времени.

## **Эндпоинты для работы с участниками и подписчиками**

### 1. **GET `/worlds/{world_id}/members`**  
**Описание**: Получение списка участников мира.  
**Что передает**:  
- `world_id` в URL.  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "world_id": 42,
    "members": [
      {
        "user_id": 1,
        "username": "User123",
        "role": "owner",
        "joined_at": "2024-11-10T08:45:00Z"
      },
      {
        "user_id": 2,
        "username": "User456",
        "role": "member",
        "joined_at": "2024-11-12T14:30:00Z"
      }
    ]
  }
  ```  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "World not found."
  }
  ```  
**Зачем нужен**: Позволяет администратору мира управлять списком участников.

---

### 2. **POST `/worlds/{world_id}/invite`**  
**Описание**: Приглашение пользователя в мир.  
**Что передает**:  
```json
{
  "email": "newuser@example.com"
}
```  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "message": "Invitation sent successfully."
  }
  ```  
- Ошибка (400 Bad Request):  
  ```json
  {
    "error": "Email is required."
  }
  ```  
**Зачем нужен**: Администратор может приглашать новых участников.

---

### 3. **GET `/worlds/{world_id}/subscribers`**  
**Описание**: Получение списка подписчиков мира (люди, которые не являются участниками, но следят за миром).  
**Что передает**:  
- `world_id` в URL.  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "world_id": 42,
    "subscribers": [
      {
        "user_id": 3,
        "username": "Follower1",
        "subscribed_at": "2024-11-15T12:00:00Z"
      },
      {
        "user_id": 4,
        "username": "Follower2",
        "subscribed_at": "2024-11-16T10:30:00Z"
      }
    ]
  }
  ```  
**Зачем нужен**: Для аналитики и работы с аудиторией мира.

---

### 4. **POST `/worlds/{world_id}/subscribe`**  
**Описание**: Подписка на мир (для пользователей, которые хотят следить за миром, но не участвуют).  
**Что передает**:  
- Параметры в запросе не требуются.  
**Что возвращает**:  
- Успех (201 Created):  
  ```json
  {
    "message": "Successfully subscribed to the world."
  }
  ```  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "World not found."
  }
  ```  
**Зачем нужен**: Для создания аудитории вокруг мира.

---

### 5. **DELETE `/worlds/{world_id}/unsubscribe`**  
**Описание**: Отмена подписки на мир.  
**Что передает**:  
- Параметры в запросе не требуются.  
**Что возвращает**:  
- Успех (204 No Content).  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "Subscription not found."
  }
  ```  
**Зачем нужен**: Позволяет пользователю отменить подписку, если он больше не хочет следить за миром.

---

## **Эндпоинты для управления ролями и правами**

### 6. **PATCH `/worlds/{world_id}/members/{user_id}/role`**  
**Описание**: Изменение роли участника мира.  
**Что передает**:  
```json
{
  "role": "moderator"
}
```  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "message": "Role updated successfully.",
    "user_id": 2,
    "new_role": "moderator"
  }
  ```  
- Ошибка (403 Forbidden):  
  ```json
  {
    "error": "You do not have permission to change roles."
  }
  ```  
**Зачем нужен**: Позволяет владельцу мира или модераторам управлять ролями участников.

---

### 7. **DELETE `/worlds/{world_id}/members/{user_id}`**  
**Описание**: Удаление участника из мира.  
**Что передает**:  
- `world_id` и `user_id` в URL.  
**Что возвращает**:  
- Успех (204 No Content).  
- Ошибка (404 Not Found):  
  ```json
  {
    "error": "Member not found."
  }
  ```  
**Зачем нужен**: Для управления участниками мира, например, удаления неактивных или нарушающих правила пользователей.

---

### 8. **GET `/worlds/{world_id}/roles`**  
**Описание**: Получение списка доступных ролей в мире.  
**Что передает**:  
- `world_id` в URL.  
**Что возвращает**:  
- Успех (200 OK):  
  ```json
  {
    "roles": [
      {
        "role": "owner",
        "permissions": ["create_channel", "delete_world", "update_settings"]
      },
      {
        "role": "moderator",
        "permissions": ["kick_member", "manage_channels"]
      },
      {
        "role": "member",
        "permissions": ["send_message", "join_channel"]
      }
    ]
  }
  ```  
**Зачем нужен**: Для отображения прав и обязанностей различных ролей в мире.

---
