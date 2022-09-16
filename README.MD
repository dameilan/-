<h1>Изменение нескольких строк бд</h1>


Для апдейта нескольких строк в базе данных вызывался dataflow из component script.

```
context.ExecuteDataflow('7e1fac12-d269-42a9-8962-4c53348039b2', UserQuestArray)
```

UserQuestArray - это питоновский дикт, который объявляется глобально и заполняется следующим образом:

```
UserQuestArray.append({'QuestionExternalId':i.Model.QuestionExternalId, 'QuestionType':"'Select'", 'isDeleted': True, 'Answer':i.Model.AnswerExternalID})
```
Так выглядит тело запроса:

![запрос](https://user-images.githubusercontent.com/16025087/190653346-3f2b95bd-5980-405f-b4a8-4c67694d5797.jpg)

Наш датафлоу для сохранения данных выглядит так: <br><br>
![dataflow](https://user-images.githubusercontent.com/16025087/190653816-d610e872-99e8-4b01-95fe-c34e3de0b186.jpg)<br>
<br>
Как видно на рисунке сначала мы используем get row model для парсинга данных. Нажатием на карандаш есть возможность редактировать "правила" для парсинга.

```
{
  "displayValue": null,
  "type": "object",
  "properties": {
    "id": {
      "PrimaryKey": true,
      "type": [
        "number",
        "null"
      ]
    },
    "data": {
      "$id": "6ae6b3ce-254c-494e-852b-fbf6d1221192",
      "displayValue": null,
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "Answer": {
            "type": [
              "string",
              "null"
            ]
          },
          "isDeleted": {
            "type": [
              "boolean",
              "null"
            ]
          },
          "QuestionType": {
            "type": [
              "string",
              "null"
            ]
          },
          "QuestionExternalId": {
            "type": [
              "string",
              "null"
            ]
          }
        }
      }
    },
    "updateDate": {
      "type": [
        "string",
        "null"
      ],
      "format": "date-time"
    },
    "createdDate": {
      "type": [
        "string",
        "null"
      ],
      "format": "date-time"
    },
    "changeAuthor": {
      "Catalog": "a34617db-3f14-4489-900e-0c1ce19b6c8e",
      "type": [
        "string",
        "integer",
        "null"
      ]
    },
    "creatorSubject": {
      "Catalog": "a34617db-3f14-4489-900e-0c1ce19b6c8e",
      "type": [
        "string",
        "integer",
        "null"
      ]
    }
  },
  "required": [
    "id",
    "createdDate",
    "updateDate",
    "creatorSubject",
    "changeAuthor",
    "data"
  ]
}
```

<br><br>
Следующим шагом мы распаковываем массив объектов с помощью extract collection, потом мы получаем ид строк для апдейта с помощью lookup reference<br><br>

![lookup](https://user-images.githubusercontent.com/16025087/190654960-57e165e9-bd52-4e6c-8e7a-1a334377486c.jpg)<br><br>

deffered update entry позволяет изменять несколько строк в бд<br><br>
![deffered update](https://user-images.githubusercontent.com/16025087/190655588-22499416-0dfc-48d5-b9e3-ed30dfc0458c.jpg)<br><br>


