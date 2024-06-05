Тестовое задание
https://docs.google.com/document/d/18V9yr-5kq_es2rOk7SOIwOv-jCJN4WlYf5fY7JFAB2w/edit#heading=h.llabv8uprm9u

Для инициации проекта запустить
make init

Для взаимодействия с приложением используется API.
Подробное описание роутов в /doc/openapi.yaml

1. Создание нового клиента.
POST: http://localhost/api/v1/clients
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

2. Изменение информации о существующем клиенте.
PATCH: http://localhost/api/v1/clients/{id}
{
    "zipCode" : "55587",
    "firstName" : "Looog",
    "lastName" : "Pddd",
    "ssn" : "123-45-6710"
}

3. Проверка возможности выдачи кредита.
http://localhost/api/v1/credit-requests:solution
{
    "clientId" : "01HZN6Q3K9ZYWGADMQFMX6ARZZ",
    "periodInMonths" : 120,
    "creditAmount" : 1000000
}

4. Выдача кредита:
POST: http://localhost/api/v1/credit-requests
{
    "clientId" : "01HZN6Q3K9ZYWGADMQFMX6ARZZ",
    "periodInMonths" : 120,
    "creditAmount" : 1000000
}


