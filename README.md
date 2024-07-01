# API V1 DOC 


### Данные аккаунтов для тестов
```json
        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"
```

## api маршрут

    https://ihor24.pythonanywhere.com/api/v1/

# fields 

`fields/<id>/<key>`

| Машрут  | Переменные | Доступные методы  | 
|-----|------------|-------------|
| fields/   | <ul><li>id(optional)</li><li>key(optional)</li></ul>| GET, POST |

 
## Описание
* GET

#### Обазательные поля
|Название поля   |Тип значения|Описание                                       |
|--------------- |------------|-----------------------------------------------|
|ID		 |ineger      |ID                                             |
|Field_Type      |string      |Тип поля                                       |
|Field_Name      |string      |Отображаемое имя                               |
|Must_Fill       |bool        |Обязательность заполнения                      |
|Basic_field     |bool        |Является ли базовой структурой                 |
|For_object_type |string      |Для какого типа объектов поле                  |
|For_structure_type |string      |Для какого типа записей поле                |
|Extra_Parameters|dict        |Набор уникальных параметров, каждого типа поля |


### Типы полей
У каждого типа поля, есть свой набор дополнительных параметров, которые передаются в Extra_Parameters.
**ОБЯЗАТЕЛЬНО** правильно устанавливать дополнительные поля, в зависимости от типа поля.

|Тип поля  |Extra_Parameters         |Описание              |Варианты значений            |
|----------|-------------------------|----------------------|-----------------------------|
|list      |List_values              |Список значений(str)  | list[str]                   |
|input     |data_type                |Тип принимаемых данных| text, integer,float         |
|          |input_type               |Тип поля для ввода    | field, area,contact_field, dual_input  |

### Описание 
* field - стандартное полее ввода
* area - поле ввода изменяемого размера
* contact_field - поле для привязки контакта
* dual_input - должен содержать два поля для ввода. Реализовуя ввод данных в формате "От" ПОЛЕ "До" ПОЛЕ. В объекте данные по такому ключу будут храниться списком ['',''].




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
	rememberme: true
	 }

  

## add 
	https://ihor24.pythonanywhere.com/api/fields/add
 `POST fields/add`

Формирование новых(пользовательских) полей, для описания карточек(объектов) происходит в клиенте.
Для заполнения структуры поля, нужно опираться на таблицу -[Структура поля](#СтруктураПоля). 
Данные нужно отправлять в **виде списка словарей**.
### Request data example
```json
[
    {
        "Field_Type": "list",
        "Field_Name": "Тестовое поле",
        "Must_fill": true,
        "Basic_field": true,
        "For_object_type": "*",
        "For_structure_type":"",
        "Extra_Parameters": {
            "List_values": ["Квартира", "До123332123м", "Земля", "Коммерческая недвижимость"]
        }
    }
    ]
```
### Response
200
```json
{
    "message": "Поле добавлено успешно"
}

```
400
```json
{
    "message": "Ошибка добавления поля: {0: {'Field_Type': ['Missing data for required field.'], 'Field_Name': ['Missing data for required field.'], 'Must_fill': ['Missing data for required field.']}}"
}

```
## updateoradd

 `POST fields/updateoradd`
 
	https://ihor24.pythonanywhere.com/api/fields/updateoradd
 
Работает по тем-же правилам что и -[add field](#add) . **НО**, принимает список из актуальных полей и новых. В случае изменения актуальных - изменяет их, а новые добавляет.


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
    "Extra_Parameters": "123",
    "For_structure_type":"123"

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
