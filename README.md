# Тестовое задание

## Описание задания

Полный текст задания доступен по ссылке: [Тестовое задание](https://docs.google.com/document/d/18V9yr-5kq_es2rOk7SOIwOv-jCJN4WlYf5fY7JFAB2w/edit#heading=h.llabv8uprm9u)

## Инициализация проекта

Для инициации проекта запустите следующую команду:

```bash
make init
```

Взаимодействие с API
Для взаимодействия с приложением используется API. Подробное описание роутов доступно в файле [/doc/openapi.yaml](https://github.com/evgenlucen/americor-test/blob/test-task/doc/openapi.yaml).

Роуты API
Создание нового клиента

```http
Copy code
POST http://localhost/api/v1/clients
Content-Type: application/json

{
    "firstName" : "second",
    "lastName" : "lastName",
    "ssn" : "123-45-6713",
    "phoneNumber" : "+15553335544",
    "email" : "test@mail.com",
    "state" : "CA",
    "street" : "Lenina",
    "city" : "SomeCity",
    "zipCode" : "55584",
    "dateOfBirth" : "1990-04-01T00:00:00+03:00"
}
```
Изменение информации о существующем клиенте

```http
Copy code
PATCH http://localhost/api/v1/clients/{id}
Content-Type: application/json

{
    "zipCode" : "55587",
    "firstName" : "Looog",
    "lastName" : "Pddd",
    "ssn" : "123-45-6710"
}
```
Проверка возможности выдачи кредита

```http
Copy code
POST http://localhost/api/v1/credit-requests:solution
Content-Type: application/json

{
    "clientId" : "01HZN6Q3K9ZYWGADMQFMX6ARZZ",
    "periodInMonths" : 120,
    "creditAmount" : 1000000
}
```
Выдача кредита

```http
Copy code
POST http://localhost/api/v1/credit-requests
Content-Type: application/json

{
    "clientId" : "01HZN6Q3K9ZYWGADMQFMX6ARZZ",
    "periodInMonths" : 120,
    "creditAmount" : 1000000
}
```
