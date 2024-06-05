# Тестовое задание

## Описание задания

Полный текст задания доступен по ссылке: [Тестовое задание](https://docs.google.com/document/d/18V9yr-5kq_es2rOk7SOIwOv-jCJN4WlYf5fY7JFAB2w/edit#heading=h.llabv8uprm9u)

## Инициализация проекта

Для инициации проекта запустите следующую команду:

```bash
make init
```

## Описание
### src/Client
Контекст Клиента.
Функция создания не содержит запроса параметра FICO. 
Расчет кредитного рейтинго (FICO) ведется с помощью обращения к внешнему API. 
Расчет FICO осуществляется асинхронно по событию ClientCreated, которое уходит в шину сообщений при успешном персисте Client.
В качестве брокера используется RabbitMq.

Допущения во имя упрощения:
1. SSN не шифруется
2. В качестве внешнего API для Scoring выступает заглушка.
3. Данные о доходе клиента не собираются. В ожидании уточнения ТЗ.
   
### src/Credit
Контекст кредитного продукта и заявок на выдачу кредитов.
#### src/CreditRequest Кредитная заявка
Заявка содержит только те данные клиента, которые нужны для расчета. 
Прямых связей с контекстом Client нет. Взаимодействие происходит через адаптеры. 
Связность модулей контролируется Deptrac
```bash
make deptrac
```
Заявки - хранимые объекты, которые фиксируют состояние важных свойств клиента на момент оформления заявки.
Происходит реальный персист в базу (Postgres).

#### src/CreditProduct
Кредитный продукт получает данные клиента и выносит решение (Solution), 
можно ли этому клиенту выдать этот кредитный продукт. 
Конфигурацию никуда не выносил, тут максимальный хардкод условий в константах. см.ниже.

Допущения во имя упрощения:
1. Разрешил CreditRequest залезть в CreditProduct во время калькуляции решения.
2. Считаем что у нас всего один кредитный продукт. Возможностей расширения множество, какой выбирать и надо-ли, нужно решать после уточнения перспектив развития проекта.

###/src/Notification
Когда выносит решение по кредитной заявке, по событию CreditRequestGotSolution создается уведомление (в контексте CreditRequest через адаптеры. Только CreditRequest регулирует контент уведомлений)
Уведомление - хранимый объект. Большую часть этого решения я принес из своего Pet проекта. 
Во имя упрощения по событию NotificationCreated мы синхронно через адаптеры идем в 
Email & SmsGateway и шлем сообщения с решением. 

###/src/Shared
Контекст с переиспользуемыми объектами и инфраструктурой. В Deptrac есть разрешение залезть в этот контекст всем остальным.

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
