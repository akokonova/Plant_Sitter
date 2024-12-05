---
title: Архитектура
hide_table_of_contents: true
---

# Архитектура


# Модель С4

## Уровень 1. Диаграмма системного контекста (System Context)
```plantuml

@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

Person(client, "Клиент", "Владелец растения")
Person(sitter, "Ситтер", "Ухаживает за растениями")
Person(support, "Сотрудник поддержки", "Обеспечивает поддержку пользователей")

System(plantSitterSystem, "Сервис по подбору ситтера для растений 'PlantSitter'", "Веб-сайт для поиска ситтеров и управления уходом за растениями")

System_Ext(paymentSystem, "Платежная система", "Обрабатывает платежи")
System_Ext(emailSystem, "Система уведомлений", "Отправляет email и SMS")

Rel(client, plantSitterSystem, "Использует для поиска ситтера и управления заказами")
Rel(sitter, plantSitterSystem, "Управляет профилем и принимает заказы")
Rel(support, plantSitterSystem, "Модерирует отзывы и помогает пользователям")

Rel(plantSitterSystem, paymentSystem, "Использует для обработки платежей")
Rel(plantSitterSystem, emailSystem, "Отправляет уведомления")

@enduml
```

## Уровень 2. Диаграмма контейнера (Container)
```plantuml

@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person(client, "Клиент", "Владелец растения")
Person(sitter, "Ситтер", "Ухаживает за растениями")
Person(support, "Сотрудник поддержки", "Обеспечивает поддержку пользователей")

System_Boundary(plantSitterSystem, "Сервис по подбору ситтера для растений 'PlantSitter'") {
    Container(webApp, "Веб-сайт", "", "Пользовательский интерфейс")
    Container(database, "База данных", "PostgreSQL", "Хранит данные пользователей, заказов и растений")
    Container(searchEngine, "Поисковая система", "", "Обеспечивает быстрый поиск ситтеров")
}

System_Ext(paymentSystem, "Платежная система", "Обрабатывает платежи")
System_Ext(emailSystem, "Система уведомлений", "Отправляет email и SMS")

Rel(client, webApp, "Использует", "HTTPS")
Rel(sitter, webApp, "Использует", "HTTPS")
Rel(support, webApp, "Использует", "HTTPS")

Rel(webApp, database, "Читает и записывает", "SQL")
Rel(webApp, searchEngine, "Выполняет поиск", "JSON/HTTPS")

Rel(webApp, paymentSystem, "Отправляет запросы на оплату", "JSON/HTTPS")
Rel(webApp, emailSystem, "Отправляет запросы на уведомления", "JSON/HTTPS")

@enduml
```

## Внешние зависимости

:::note
Тут будет табличка с описанием интеграций
:::

| Сервис | Тип интеграции | Описание |
| ------ | -------------- | -------- |
| 1      | 2              |          |