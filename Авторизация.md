### Данные аккаунтов для тестов
```json
        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"
```

## API маршрут

    https://ihor24.pythonanywhere.com/api/v1/

## login 
`POST /login`

При каждом запросе нужно отправлять токен авторизации блоке headers, ключ должен называться `Authorization`. 
Для получения токена необходимо отправить POST запрос на логин и прикрепить данные для авторизации пользователя. В случае ошибки на сервере будет возвращен код `400` и код `401` если данные не правильные. 
Так же код `401` будет возвращаться при истечении токена или ошибки в нем, при каждом запросе.
Пример данных 

```json
{
    "name":"login_alexander",
    "password":"AxpPlt2024"
}
```

Ответ сервера:
200
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2xvZ2luIjoibG9naW5fYWxleGFuZGVyIiwiZXhwIjoxNzIwMTY5ODI1fQ.Dg-MH_BvQyqT84ccFOuvBKpzXwcM5kfTGLYZ1PiKTlQ"
}
```
