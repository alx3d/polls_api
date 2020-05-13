# polls_api
# API для системы опросов пользователей

## Инструкция по разворачиванию приложения (локально)


## Документация по API
Взаимодействие с API приложения осуществляется путем отправки GET (POST, PUT, PATCH, DELETE) запросов .

### Опросы
**Для добавление, редактирования, удаления опросов необходимы права администратора.**
- Вывод списка всех опросов:
GET-запрос к `<api/polls>`
- Добавления опроса:
POST-запрос к `<api/polls>` вида:
```
  {
    "title": "Наименование", // наименование
    "description": "Описание", // описание
    "date_end": "2020-05-14" // дата окончания
  }
```
*Поле date_start (Дата начала) заполняется автоматически при создании опроса и не подлежит редактированию в дальнейшем.*
- Редактирование опроса:
PUT(PATCH)-запрос к `<api/polls/$>` (где $ - идентификатор опроса), вида:
```
  {
    "title": "Наименование",
    "description": "Описание",
    "date_end": "2020-05-14"
  }
```
- Удаление опроса:
DELETE-запрос к `<api/polls/$>` (где $ - идентификатор опроса)