# polls_api
# API для системы опросов пользователей

## Инструкция по развертыванию приложения (локально)
**Для разворачивания приложения необходимо установить зависимости из файла requirements.txt.**
1. Находясь в корне Django-проекта последовательно в консоли выполнить команды:
```
python manage.py migrate  # выполнение миграций, создание (на основе имеющихся моделей) таблиц в БД
python manage.py createsuperuser  # создание пользователя с правами администратора
python manage.py runserver  # запуск сервера разработки
```
2. Для проверки работы приложения, перейти по ссылке: http://127.0.0.1:8000/api/
3. Аутентификация (в браузере):
URL `<api-auth/login>`
4. Выход (в браузере):
URL `<api-auth/logout>`
5. Все запросы к URL, указанным в документации, выполняются по следующей схеме авторизации:
- в качестве ключа `<Authorization>` вместе с запросом, требующим идентификации пользователя, в режиме (Type) Basic Auth передаются его Username и Password
- в качестве ключа `<Content-Type>` вместе с телом запроса передается значение 'application/json'.


## Документация по API
*Взаимодействие с API приложения осуществляется путем отправки GET (POST, PUT, PATCH, DELETE) запросов.*

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
    "title": "Наименование", // наименование
    "description": "Описание", // описание
    "date_end": "2020-05-14" // дата окончания
  }
```
- Удаление опроса:
DELETE-запрос к `<api/polls/$>` (где $ - идентификатор опроса)

### Вопросы
**Для добавление, редактирования, удаления вопросов необходимы права администратора.**
- Добавления вопроса:
POST-запрос к `<api/questions>` вида:
```
  {
    "text": "Текст вопроса", // текст вопроса
    "type": 0, // тип вопроса: 0 - 'Текст', 1 - 'Выбор одного варианта', 2 - 'Выбор из нескольких вариантов'
    "poll": 1 // идентификатор опроса, к которому относится данный вопрос из 'api/polls'
  }
```
- Редактирование вопроса:
PUT(PATCH)-запрос к `<api/questions/$>` (где $ - идентификатор вопроса), вида:
```
  {
    "text": "Текст вопроса", // текст вопроса
    "type": 0, // тип вопроса: 0 - 'Текст', 1 - 'Выбор одного варианта', 2 - 'Выбор из нескольких вариантов'
    "poll": 1 // идентификатор опроса, к которому относится данный вопрос из 'api/polls'
  }
```
- Удаление вопроса:
DELETE-запрос к `<api/questions/$>` (где $ - идентификатор вопроса)

### Активные опросы
- Получение списка активных опросов (дата окончания которых не позднее текущего дня, содержащие один и более вопросов):
GET-запрос к `<api/active_polls>`
*Список активных опросов доступен только для чтения*
- Получение конкрентного активных опроса:
GET-запрос к `<api/active_polls/$>` (где $ - идентификатор опроса)
*В поле 'questions' опросов содержится список вопросов, относящихся к опросу.*

### Сохранение ответов пользователя
Сохранение ответов пользователей выполняется путем POST-запроса к `<api/answers/>`, вида:
```
  {
    "answer_text": "Текст ответа", // ответ пользователя
    "user": 2, // идентификатор пользователя
    "question": 1 // идентификатор вопроса, на который отвечает пользователь
  }
```
*В случае попытки дать повторный ответ на ранне отвеченный вопрос, в качестве ответа на запрос вернется ошибка:*
```
  {
    "non_field_errors": [
        "Вы уже отвечали на данный вопрос"
    ]
  }
```

### Завершенные опросы
- Получение списка завершенных опросов:
GET-запрос к `<api/completed_polls>`
*Список завершенных опросов доступен только для чтения*
*В поле 'question_answer' списка вопросов каждого опроса содержится ответ пользователя.*
