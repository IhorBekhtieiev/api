# API DOC V1
### Данные аккаунтов для тестов
```json
        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"
```

## api

    https://ihor24.pythonanywhere.com/api/

# REST API

Снизу будут примеры запросов и их ответы

## Get


### fields 
`GET /fields`

	https://ihor24.pythonanywhere.com/api/object/fields
 
### Описание

Унифицированный запрос, возвращающий поля необходимы для заполнения карточек ***ЛЮБОГО*** типа.
**object**/fields, **contact**/fields и т.д. Каждое поле имеет список из обязательных параметров и список Extra_Parameters. В котором в виде словаря записаны все параметры, которые уникальны для данного поля.
Базовая структура - поля, которые определены системой, их нельзя удалить.
**For_object_type** - помимо явного указания на тип объекта, может содержать "*". В таком случае поле применяется ко всем объектам.

#### Обазательные поля
|Название поля  |Тип значения|Описание                      |
|---------------|------------|------------------------------|
|Field_Type     |string      |Тип поля                      |
|Field_Name     |string      |Отображаемое имя              |
|Must_Fill      |bool        |Обязательность заполнения     |
|Basic_field    |bool        |Является ли базовой структурой|
|For_object_type|string      |Для какого типа объектов поле |

### Типы полей
|Тип поля  |Extra_Parameters         |Описание              |Варианты значений            |
|----------|-------------------------|----------------------|-----------------------------|
|list      |List_values              |Список значений(str)  | *                           |
|input     |data_type                |Тип принимаемых данных| text, integer,float         |
|          |input_type               |Тип поля для ввода    | field, area,contact_field   |




#### Пример ответа

```json
[
	{
        "Basic_field": true,
        "Extra_Parameters": {
            "List_values": [
                "Квартира",
                "Дом",
                "Земля",
                "Коммерческая недвижимость"
            ]
        },
        "Field_Name": "Тип объекта",
        "Field_Type": "list",
        "For_object_type": "*",
        "ID": 1,
        "Must_fill": true
    },
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "data_type": "text",
            "input_type": "field"
        },
        "Field_Name": "Название объекта",
        "Field_Type": "input",
        "For_object_type": "*",
        "ID": 2,
        "Must_fill": true
    }
]
```






### loginform

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
### getme *возвращает информацию про текущего пользователя*
`logined only`
`GET /users/getme`

    https://ihor24.pythonanywhere.com/api/users/getme

### Response

	{
        'Login': user login,
        'Name': display name,
        'Phone_numbers': list of phone numbers,
        'Contact_fields': возвращает пользовательские поля. ПОКА ЧТО ИГНОРИРУЕМ
        }
### test

`GET /test`

    https://ihor24.pythonanywhere.com/api/test

### Response
	 Вернет результат только если пользователь залогинен
	{'data': 'Hello' + username}
	 Логин сессионный, храниться в куках
	 
 ### navitems | **УСТАРЕЛ**
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

### login

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
