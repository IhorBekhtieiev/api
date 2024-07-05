
# API V1 DOC 


### Данные аккаунтов для тестов
```json
        "Username": "login_alexander"
        "Password": "AxpPlt2024"

        "Username": "login_ihor"
        "Password": "Dolphin2024"
```

## API маршрут

    https://ihor24.pythonanywhere.com/api/v1/

# Авторизация
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

# fields 
Маршрут со всеми доступными переменными. 
***Использовать только в таком порядке!***
`fields/<id>/<key>`


| Маршрут  | Переменные | Доступные методы  | 
|-----|------------|-------------|
| fields/   | <ul><li>id(optional)</li><li>key(optional)</li></ul>| GET, POST, -[DELETE](#DELETE) |

 
## Описание
### GET 
+ ```fields/```
Вернет все поля, которые на данный момент находятся в базе данных.
Каждое поле имеет обязательные атрибуты и дополнительные (*Extra_Parameters*). 
У каждого типа поля есть свои, иногда уникальные, обязательные дополнительные атрибуты, описывающие какие-то параметры конкретного поля. 
 **For_structure_type** имеет свои обязательные параметры, в зависимости от принадлежности поля к тому или иному виду шаблона(*контакт, объект недвижимости, заявка*). 
 
+ ```fields/<id>```
Вернет list из одного объекта. Которому принадлежит id.  
+ ```fields/<id>/<key>```
Вернет значение атрибута key в поле с id. В формате `{key:attr}`

### Обязательные поля
|Название поля   |Тип значения|Описание                                       |
|--------------- |------------|-----------------------------------------------|
|ID		 |ineger      |ID                                             |
|Field_Type      |string      |Тип поля                                       |
|Field_Name      |string      |Отображаемое имя                               |
|Must_Fill       |bool        |Обязательность заполнения                      |
|Basic_field     |bool        |Является ли базовой структурой                 |
|For_structure_type |list[string]|Для какого типа записей поле. ["*"] если для всех          |
|Extra_Parameters|dict        |Набор уникальных параметров, каждого типа поля |

* `For_structure_type` - Если стоит в значении для всех, то в `Extra_Parameters` поля, стоит учитывать дополнительные параметры всех видов шаблонов.
* `типы объектов недвижимости, относится к object` - Все актуальные типы объектов, находятся в поле под **ID=1**, это поле типа list. В атрибуте **List_values** находятся все типы объектов, актуальные на момент вызова. Так же можно воспользоваться GET запросом `/api/v1/objects/types/`, который сразу вернет список типов объектов, в виде
```json
{
    "types": ["Квартира","Дом","Земля","Коммерческая недвижимость"]
}
```

### Типы шаблонов и их дополнительные параметры
|Тип шаблона|Extra_Parameters         |Описание              |Варианты значений            |
|----------|-------------------------|----------------------|-----------------------------|
|object    |For_object_type          | К каким шаблонам относится поле объекта<br>["*"] если для всех   | list[str]                   |


### Типы полей
У каждого типа поля, есть свой набор дополнительных параметров, которые передаются в Extra_Parameters.
**ОБЯЗАТЕЛЬНО** правильно устанавливать дополнительные поля, в зависимости от типа поля.

|Тип поля  |Extra_Parameters         |Описание              |Варианты значений            |
|----------|-------------------------|----------------------|-----------------------------|
|list      |List_values              |Список значений(str)  | list[str]                   |
|input     |data_type                |Тип принимаемых данных| text, integer,float         |
|          |input_type               |Тип поля для ввода    | field, textarea,contact_field, dual_input  |



### Описание 
* `field` - стандартное полее ввода

* `area` - поле ввода изменяемого размера
* `contact_field` - поле для привязки контакта
* `dual_input` - должен содержать два поля для ввода. Реализовывая ввод данных в формате "От" ПОЛЕ "До" ПОЛЕ. В объекте данные по такому ключу будут храниться списком ['',''].




#### Пример ответов на GET запросы 
* 200
`api/v1/fields`
```json
[
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "For_object_type": ["*"],
            "List_values": [
                "Квартира",
                "Дом",
                "Земля",
                "Коммерческая недвижимость"
            ]
        },
        "Field_Name": "Тип объекта",
        "Field_Type": "list",
        "For_structure_type": ["*"],
        "ID": 1,
        "Must_fill": true
    },
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "For_object_type": ["*"],
            "data_type": "text",
            "input_type": "field"
        },
        "Field_Name": "Название объекта",
        "Field_Type": "input",
        "For_structure_type": ["*"],
        "ID": 2,
        "Must_fill": true
    }
]
```



`api/v1/fields/1`

```json
[
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "For_object_type": ["*"],
            "List_values": [ "Квартира","Дом","Земля","Коммерческая недвижимость" ]
        },
        "Field_Name": "Тип объекта",
        "Field_Type": "list",
        "For_structure_type": ["*"],
        "ID": 1,
        "Must_fill": true
    }
]
```

`api/v1/fields/1/Field_Name`
```json
{
"Field_Name": "Тип объекта"
}
```

* 400

`api/v1/fields/500`


`api/v1/fields/str`
`api/v1/fields/5str`


```json 
{
"message": "Поле не найдено"
}
```
```json
{
"message": "Неверный формат ID(string). ID должен быть ineger"
}
```

`/api/v1/fields/1/qwe`
```json
{
"message": "Ошибка получения данных: 'StructureFields' object has no attribute 'qwe'"
}
```
### POST
Принимает список из словарей, аналогичный списку получаемому на GET запрос. 
Для формирования новых полей, нужно опираться на таблицы из метода -[GET](#GET).
В списке могут быть старые поля, с измененными атрибутами и новые. Сервер автоматически обновит существующие поля и добавит новые.
**ВАЖНО** в существующих полях должен быть атрибут **ID**, а в новых - **нет**. 
`400` - при ошибке, `200` - успешно.

Пример запроса:
```json
[
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "For_object_type": [
                "*"
            ],
            "List_values": [
                "Квартира",
                "Дом",
                "Земля",
                "Коммерческая недвижимость"
            ]
        },
        "Field_Name": "Тип объекта",
        "Field_Type": "list",
        "For_structure_type": [
            "*"
        ],
        "ID": 1,
        "Must_fill": true
    },
    {
        "Basic_field": true,
        "Extra_Parameters": {
            "For_object_type": [
                "*"
            ],
            "data_type": "text",
            "input_type": "field"
        },
        "Field_Name": "Название объекта",
        "Field_Type": "input",
        "For_structure_type": [
            "*"
        ],
        "Must_fill": true
    }
]
```
В данном примере первое поле будет изменено, второе добавлено. 
### DELETE
Удаляет поля из бд, по айди. 'IDs' - ключ, под которым нужно передавать список из ID объектов  для удаления.
Пример запроса:
`400` - при ошибке, `200` - успешно.
```json
{
    "IDs" : [19,18]
}
```

-[DELETE](#DELETE)

QWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWE

QWEQEQWE
QWEQEQWEQWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWEQWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWE
QWEQEQWEQWEQEQWE
QWEQEQWE



### DELETE
