---
title: Группы товаров и группы модификаций
permalink: rest_products_groups.html
sidebar: evorest
product: REST API
---

Запросы к Облаку Эвотор для управления группами товаров и группами модификаций.

## Создать групп(ы)

Создает новую группу или группу модификаций в магазине. Идентификаторы объектов формирует Облако.

Можно создать как один элемент за раз, так и несколько (до 1000).

Для выполнения множественных операций - указывайте `Content-Type` с модификатором `+bulk`.

    POST /stores/{store-id}/product-groups

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`name`| `string ` | **Обязательный**. Название экземпляра сущности, длиной не менее 1 символа и не более 128 символов.
`parent_id`| `string` | Уникальный идентификатор группы или группы модификаций, к которой иерархически привязана группа или группа модификаций. Если элемент номенклатуры находится в корне иерархии, то данное свойство не указывается.
`barcodes`| `Array of string` |  Массив штрихкодов для группы модификаций. Не применим для группы, которая содержит элементы, отличные от модификаций товара.
`attributes`  | `object` | Объект, содержащий перечисление возможных характеристик и их значений для модификаций в группе. Наличие ключа attributes для группы означает что эта группа является "Группой модификаций" и все дочерние элементы этой группы являются "Модификациями товара". Не применим для группы, которая содержит элементы, отличные от модификаций товара.

### Пример тела запроса

```json
[
  {
    "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
    "name": "Группа",
    "barcodes": [
      "2000000000060"
    ],
    "attributes": {}
  }
]
```

### Ответ

```
200 Успешно
```

```json
{
  "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
  "name": "Группа",
  "barcodes": [
    "2000000000060"
  ],
  "attributes": {}
}
```

```
202 Успешно
```

```json
{
  "id": "ca187ddc-8d1b-4d0e-b20d-c509082da528",
  "modified_at": "2018-01-01T00:00:00.000Z",
  "status": "FAILED",
  "type": "product",
  "details": [
    {
      "message": "validation failed for specified payload",
      "code": "validation_failed",
      "product_id": "1009017d-5f3d-422a-964b-3ff11ee2d183",
      "violations": [
        {
          "reason": "must not be null",
          "subject": "name"
        }
      ]
    }
  ]
}
```

## Создать/заменить несколько групп

Создает или заменяет группы товаров или группы модификации в магазине. Идентификаторы объектов формирует клиент API.

Можно передать за раз до 1000 элементов.

    PUT /stores/{store-id}/product-groups

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`name`| `string ` | **Обязательный**. Название экземпляра сущности, длиной не менее 1 символа и не более 128 символов.
`parent_id`| `string` | Уникальный идентификатор группы или группы модификаций, к которой иерархически привязана группа или группа модификаций. Если элемент номенклатуры находится в корне иерархии, то данное свойство не указывается.
`barcodes`| `Array of string` |  Массив штрихкодов для группы модификаций. Не применим для группы, которая содержит элементы, отличные от модификаций товара.
`attributes`  | `object` | Объект, содержащий перечисление возможных характеристик и их значений для модификаций в группе. Наличие ключа attributes для группы означает что эта группа является "Группой модификаций" и все дочерние элементы этой группы являются "Модификациями товара". Не применим для группы, которая содержит элементы, отличные от модификаций товара.

### Пример тела запроса

```json
[
  {
    "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
    "name": "Группа",
    "barcodes": [
      "2000000000060"
    ],
    "attributes": {}
  }
]
```

### Ответ

```
202 Успешно
```

```json
{
  "id": "ca187ddc-8d1b-4d0e-b20d-c509082da528",
  "modified_at": "2018-01-01T00:00:00.000Z",
  "status": "FAILED",
  "type": "product",
  "details": [
    {
      "message": "validation failed for specified payload",
      "code": "validation_failed",
      "product_id": "1009017d-5f3d-422a-964b-3ff11ee2d183",
      "violations": [
        {
          "reason": "must not be null",
          "subject": "name"
        }
      ]
    }
  ]
}
```

## Получить все группы

Возвращает все группы из магазина.

    GET /stores/{store-id}/product-groups

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`since` | `long` | Фильтр по дате изменения группы (updated_at). Полезен, если полная синхронизация уже была произведена ранее и требуется получить только измененные с конкретного момента времени экземпляры групп. Указывается только в первом запросе из серии постраничных запросов. Если требуется получить полный список групп, - не указывайте данный параметр.
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных. Указывается для получения второй и последующих страниц данных. Для корректного выполнения постраничной навигации должен принимать значение переменной next_cursor, присутсвовавшее в предыдущем ответе от Облака Эвотор.

### Ответ

```
200 Success
```

```json
{
  "items": [
    {
      "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
      "name": "Группа",
      "barcodes": [
        "2000000000060"
      ],
      "attributes": {}
    }
  ],
  "paging": {
    "next_cursor": "string"
  }
}
```

## Удалить несколько групп

Удаляет группы из магазина.

Чтобы удалить несколько групп, укажите через запятую идентификаторы элементов к удалению в параметре `id`.

Можно удалить до 100 элементов за раз.

    DELETE /stores/{store-id}/product-groups

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`id`| `string` | Идентификатор группы в Облаке Эвотор.

### Ответ

```
204 Успешно
```


## Создать/заменить товар

Создает или заменяет в магазине один товар или модификацию товара с указанным идентификатором.

    PUT /stores/{store-id}/product-groups/{product-group-id}


### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`name`| `string ` | **Обязательный**. Название экземпляра сущности, длиной не менее 1 символа и не более 128 символов.
`parent_id`| `string` | Уникальный идентификатор группы или группы модификаций, к которой иерархически привязана группа или группа модификаций. Если элемент номенклатуры находится в корне иерархии, то данное свойство не указывается.
`barcodes`| `Array of string` |  Массив штрихкодов для группы модификаций. Не применим для группы, которая содержит элементы, отличные от модификаций товара.
`attributes`  | `object` | Объект, содержащий перечисление возможных характеристик и их значений для модификаций в группе. Наличие ключа attributes для группы означает что эта группа является "Группой модификаций" и все дочерние элементы этой группы являются "Модификациями товара". Не применим для группы, которая содержит элементы, отличные от модификаций товара.

### Пример тела запроса

```json
{
  "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
  "name": "Группа",
  "barcodes": [
    "2000000000060"
  ],
  "attributes": {}
}
```

### Ответ

```
200 Успешно
```

## Получить группу

Возвращает из магазина группу товаров или группу модификаций с указанным идентификатором.

    GET /stores/{store-id}/product-groups/{product-group-id}

### Ответ

```
200 Успешно
```

```json
{
  "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
  "name": "Группа",
  "barcodes": [
    "2000000000060"
  ],
  "attributes": {}
}
```

## Обновить группу

Обновляет название группы.

    PATCH /stores/{store-id}/product-groups/{product-group-id}

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`name`| `string` | Название группы.

### Пример тела запроса

```json
{
  "name": "Группа товаров"
}
```

### Ответ

```
200 Успешно
```

## Удалить товар

Удаляет из магазина группу с указанным идентификатором.

    DELETE /stores/{store-id}/product-groups/{product-group-id}

### Ответ

```
204 Успешно
```