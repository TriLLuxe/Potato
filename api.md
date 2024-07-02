# API Документация для "Картошка"

В качестве ответа возвращается application/json

В качестве тела запроса используется application/json

## Сессия
### Создание сессии
POST /api/sessions
Body:
```
{
  "phoneNumber": "string",
  "password": "string"
}
```
Response:
При http status 200
```
{
  "token": "string",
  "userId": "integer",
  "expiresAt": "string"
}
```
При http status 400:
```
"code"=1,
"text": "Некоректные данные номера телефона",
"code"=2,
"text": "Некорректный пароль"
```
1-проблемы с телефоном

2-проблемы с паролем 
### Получение информации о сессии
GET /api/sessions/{token}
>token (string): Токен сессии.

Response:
```
{
  "token": "string",
  "userId": "integer",
  "createdAt": "string",
  "expiresAt": "string"
}
```
### Выход из сессии
DELETE /api/sessions/{token}
>token (string): Токен сессии.

Response:

 При http status 200: `No Content`  
## Пользователи
### Cоздание пользователя
POST /api/users  

#### Body:
```
{
  "lastName": "string",
  "firstName": "string",
  "middleName": "string",
  "phoneNumber": "string",
  "email": "string",
  "password": "string",
  "birthDate": "string"
}
```

- lastName - Фамилия пользователя (строка, от 1 до 50 символов).

- firstName - Имя пользователя (строка, от 1 до 50 символов).

- middleName - Отчество пользователя (опционально, строка, от 0 до 50 символов).

- phoneNumber - Номер мобильного телефона пользователя (строка, ровно 11 цифр, начинается с '7').

- email - Адрес электронной почты пользователя (строка, стандартный формат).

- password - Пароль пользователя (строка, от 8 до 64 символов, только латинские буквы, цифры, знаки !?).

- birthDate - Дата рождения пользователя (строка, формат ISO 8601).

 Response:
```
{
  "id": "integer",
  "lastName": "string",
  "firstName": "string",
  "middleName": "string",
  "phoneNumber": "string",
  "email": "string",
  "birthDate": "string"
}
```
### Получение информации о пользователе

GET /api/users/{id}
>id (integer): Идентификатор пользователя.

Response:
```
{
  "id": "integer",
  "lastName": "string",
  "firstName": "string",
  "middleName": "string",
  "phoneNumber": "string",
  "email": "string",
  "birthDate": "string",
  "createdAt": "string",
  "updatedAt": "string"
}
```
### Редактирование пользователя

PUT /api/users/{id}
>id (integer): Идентификатор пользователя.

Body:
```
{
  "lastName": "string",
  "firstName": "string",
  "middleName": "string",
  "birthDate": "string"
}
```
## Кошелек
### Получение информации о кошельке
GET /api/wallets/{userId}
>userId (integer): Идентификатор пользователя.

Response:
```
{
  "userId": "integer",
  "balance": "integer"
}
```
## Счет на оплату 
### Создание счета на оплату

POST /api/bills

Body:
```
{
  "amount": "integer",
  "senderId": "integer",
  "recipientId": "integer",
  "comment": "string",
  "date": "string"
}
```
- comment - комментарий не более 250 символов, строка произвольная. Дата выставления счёта в ISO 8601 формате.

### Отмена счета на оплату
DELETE/api/bills/{UUID}
>UUID: Идентификатор счета на оплату.

Response:

 При http code 200: `OK`

 ### Оплата счета на оплату
PUT /api/bills/{UUID}/pay
>UUID: Идентификатор счета на оплату.

Response:

При http code 200: `OK`

При http code 400: 
```
"code": 1,
"text": "Invalid identifier"
```
1 - некорректный идентификатор счета на оплату
### Получение информации о счете на оплату
GET /api/bills/{UUID}
>UUID: Идентификатор счета на оплату.

Response:
```
{
  "billId": "string",
  "amount": "integer",
  "senderId": "integer",
  "recipientId": "integer",
  "comment": "string",
  "status": "string",
  "createdAt": "string"
}
```

### Получение всех выставленных счетов
GET /api/bills/sent/{userId}
>userId (integer): Идентификатор пользователя.
Response:
```
 {
    "billId": "string",
    "amount": "integer",
    "recipientId": "integer",
    "comment": "string",
    "status": "string",
    "createdAt": "string"
  }
```
### Получение всех счетов к оплате
GET /api/bills/received/{userId}
>userId (integer): Идентификатор пользователя.
Response:
```
{
    "billId": "string",
    "amount": "integer",
    "senderId": "integer",
    "comment": "string",
    "status": "string",
    "createdAt": "string"
  }
```
## Денежные переводы
### Создание денежного перевода
POST /api/transactions

Body:
```
{
  "amount": "integer",
  "senderId": "integer",
  "recipientId": "integer",
  "type": "string"
}
```
Response:

При http code 200: `Created`

При http code 400:
```
"code": 1,
"text": "Check the amount field",
"code": 2,
"text": "invalid recipient id"
```
1 - некорректная число в поле "сумма"

2 - некорренкный id получателя 
### Получение информации о денежном переводе
GET /api/transactions/{transactionId}
>transactionId (UUID): Идентификатор денежного перевода.

Response:
```
{
  "transactionId": "string",
  "amount": "integer",
  "senderId": "integer",
  "recipientId": "integer",
  "type": "string",
  "createdAt": "string"
}
```
### Получение истории переводов
GET /api/transactions/history/{userId}
>userId (integer): Идентификатор пользователя.

Filters:
- type (string): Тип перевода (входящий или исходящий).
- status (string): Статус перевода (оплачен или не оплачен).
- recipientId (integer): Идентификатор получателя

Response:
```
  {
    "transactionId": "string",
    "amount": "integer",
    "senderId": "integer",
    "recipientId": "integer",
    "type": "string",
    "createdAt": "string"
  }
```

