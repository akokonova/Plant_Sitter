---
title: Логическая схема модели данных
sidebar_position: 2
---
# Логическая схема модели данных

```plantuml

@startuml Логическая схема
!define TABLE class

TABLE users {
  id: INTEGER <<PK>>
  name: VARCHAR
  email: VARCHAR
  phone: VARCHAR
  photo: VARCHAR
  city: VARCHAR
  role: ENUM
  created_at: TIMESTAMP
}

TABLE plants {
  id: INTEGER <<PK>>
  owner_id: INTEGER <<FK>>
  name: VARCHAR
  description: TEXT
  care_instructions: TEXT
  created_at: TIMESTAMP
}

TABLE orders {
  id: INTEGER <<PK>>
  client_id: INTEGER <<FK>>
  sitter_id: INTEGER <<FK>>
  status: ENUM
  order_date: TIMESTAMP
  address: TEXT
  total_price: DECIMAL
  created_at: TIMESTAMP
}

TABLE order_plants {
  order_id: INTEGER <<PK, FK>>
  plant_id: INTEGER <<PK, FK>>
}

TABLE reviews {
  id: INTEGER <<PK>>
  order_id: INTEGER <<FK>>
  author_id: INTEGER <<FK>>
  rating: INTEGER
  comment: TEXT
  created_at: TIMESTAMP
}

TABLE support_tickets {
  id: INTEGER <<PK>>
  user_id: INTEGER <<FK>>
  message: TEXT
  status: ENUM
  created_at: TIMESTAMP
}

users ||--o{ plants
users ||--o{ orders
users ||--o{ reviews
users ||--o{ support_tickets
orders ||--o{ order_plants
plants ||--o{ order_plants
orders ||--o| reviews

@enduml


