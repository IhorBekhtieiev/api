# API DOC V1

# Оглавление
|Тип обращения  |Запросы                        |
|---------------|-------------------------------|
|-[GET](#GET)   | -[fields](#fields)            |
|               | -[getme](#getme)              |
|-[POST](#POST) | -[POST](#login)               |
|               | -[strucfchek](#strucfchek)    |
|               | -[addfield](#addfield)        |


### Данные аккаунтов для тестов
```json
        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"
```

## api маршрут

    https://ihor24.pythonanywhere.com/api/

# GET


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

## login

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
## test 
запросы для проверок 
	https://ihor24.pythonanywhere.com/api/test/ENDPOINT
 
### strucfchek
`POST test/strucfchek`

	https://ihor24.pythonanywhere.com/api/test/strucfchek
 
#### Описание
Проверка правильности структуры поля. **Не проводит валидацию данных или проверку типов данных**.
**ВАЖНО** данные нужно отправлять **В ВИДЕ СПИСКА**
В разработке валидация данных и полная проверка данных этим запросом.

#### Request data example
Пример запроса, ответом на который будет **200**, так-как в данном случае структура не нарушена.
```json
[
{
    
    "ID": 0,
    "Field_Type": "123",
    "Field_Name": "123",
    "Must_Fill": "123",
    "Basic_field": "123",
    "For_object_type": "123",
    "Extra_Parameters": "123"

}
]
```


#### Response
400 
```json
{
    "message": "Ошибка в поле (Extra_Parmeters), ID:(0).Ошибка в поле (Must_will), ID:(1).Ошибка в поле (Extra_Parmeters), ID:(1)."
}
```
200
```json
{
    "message": "Структура данных корректна"
}
```
