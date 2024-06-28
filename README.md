# API DOC V1
### Данные аккаунтов для тестов

        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"


## api

    https://ihor24.pythonanywhere.com/api/

# REST API

Снизу будут примеры запросов и их ответы

## Get

### Request

`GET /loginform`

    https://ihor24.pythonanywhere.com/api/loginform

### Response

	   {
	  "LoginForm": {
		"DisplayName": "Ваш логин",
		"FieldType": "input"
	  },
	  "PasswordForm": {
		"DisplayName": "Ваш пароль",
		"FieldType": "input"
	  },
	  "RememberMe": {
		"DisplayName": "Запомнить меня",
		"FieldType": "checkbox"
	  },
	  "SingInbutton": {
		"DisplayName": "Войти",
		"FieldType": "button"
	  }
	}
### Request

`GET /test`

    https://ihor24.pythonanywhere.com/api/test

### Response
	 Вернет результат только если пользователь залогинен
	{'data': 'Hello' + username}
	 Логин сессионный, храниться в куках
	 
 ### Request get navigation items
`logined only`

`GET /navitems`

    https://ihor24.pythonanywhere.com/api/navitems

### Response structure
	{
		itemName(string) : url(string)
	}
### Response example
	{
		"Заявки": "lids",
		"Контакты": "contacts",
		"Настройки": "settings",
		"Объекты": "objects"
	}
	
	
	
	
# POST
## Логин

### Request

`POST /login/`

	 https://ihor24.pythonanywhere.com/api/login/

### Response
	
	"Неправильный логин или пароль", 401
	или
	"message": "Успешный вход", 200

#### Login data
	{
	name: 'Ihor',
	password: 'password',
	rememberMe: true
	 }
