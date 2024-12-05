---
title: Sequence diagram
sidebar_position: 2
---
Sequence diagram

```plantuml

@startuml
actor "Владелец растения" as owner
participant "Веб-интерфейс" as ui
participant "Сервис подбора" as service
database "База данных" as db
participant "Ситтер" as sitter

owner -> ui: Запрос на поиск ситтера
activate ui

ui -> service: Передача параметров поиска
activate service

service -> db: Запрос подходящих ситтеров
activate db
db --> service: Список подходящих ситтеров
deactivate db

service --> ui: Отображение списка ситтеров
deactivate service

ui --> owner: Показ списка доступных ситтеров

owner -> ui: Выбор ситтера
ui -> service: Запрос на бронирование
activate service

service -> sitter: Запрос на подтверждение бронирования
activate sitter

alt Ситтер ответил на запрос
    sitter --> service: Ответ на подтверждение запроса
    deactivate sitter
else Ситтер не отвечает в течение суток
    sitter --> service: тайм-аут ожидания ответа
end

alt Ситтер подтвердил запрос
    service -> db: Сохранение заказа
    activate db
    db --> service: Подтверждение сохранения
    deactivate db
    service --> ui: Подтверждение бронирования
    ui --> owner: Уведомление об успешном заказе
else Ситтер отклонил запрос или не ответил
    service --> ui: Уведомление о недоступности
    ui --> owner: Предложение выбрать другого ситтера
    owner -> ui: Выбор другого ситтера
end

deactivate service
deactivate ui

@enduml


```

## Описание алгоритма

1. **Owner** отправляет запрос на поиск ситтера по определнным параметрам.
2. **System** обрабатывает данные и выдает список похдходящий под параметры ситтеров.
3. **Owner** выбирает ситтера из полученного списка и отправляет запрос на бронирование.
4. **System** обрабатывает выбор клиента и направляет запрос на подтверждение бронирования ситтеру.
   - Если ситтер подтвердил бронирование:
     - **System** сохраняет данные в **Database**.
     - **System** отправляет **Owner**: `Уведомление об успешно созданном бронировании`.
   - Если ситтер отклонил бронирование или не отвечал:
     - **System** отправляет **Owner**: `Уведомление о недоступности ситтера`.