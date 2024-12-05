---
title: Физическая схема модели данных
sidebar_position: 3
---
# Физическая схема модели данных

```plantuml

@startuml Физическая схема
!define TABLE class

TABLE users {
  id: SERIAL <<PK>>
  name: VARCHAR(100) NOT NULL
  email: VARCHAR(255) NOT NULL UNIQUE
  phone: VARCHAR(20)
  photo: VARCHAR(255)
  city: VARCHAR(100)
  role: VARCHAR(10) CHECK (role IN ('CLIENT', 'SITTER', 'SUPPORT'))
  created_at: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
}

TABLE plants {
  id: SERIAL <<PK>>
  owner_id: INTEGER NOT NULL <<FK users(id)>>
  name: VARCHAR(100) NOT NULL
  description: TEXT
  photo: VARCHAR(255)
  care_instructions: TEXT NOT NULL
  created_at: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
}

TABLE orders {
  id: SERIAL <<PK>>
  client_id: INTEGER NOT NULL <<FK users(id)>>
  sitter_id: INTEGER NOT NULL <<FK users(id)>>
  status: VARCHAR(20) CHECK (status IN ('NEW', 'IN_PROGRESS', 'COMPLETED'))
  order_date: TIMESTAMP NOT NULL
  address: TEXT NOT NULL
  total_price: DECIMAL(10,2) NOT NULL CHECK (total_price > 0)
  created_at: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
}

TABLE order_plants {
  order_id: INTEGER <<PK, FK orders(id)>>
  plant_id: INTEGER <<PK, FK plants(id)>>
  PRIMARY KEY (order_id, plant_id)
}

TABLE reviews {
  id: SERIAL <<PK>>
  order_id: INTEGER NOT NULL UNIQUE <<FK orders(id)>>
  author_id: INTEGER NOT NULL <<FK users(id)>>
  rating: INTEGER NOT NULL CHECK (rating BETWEEN 1 AND 5)
  comment: TEXT
  created_at: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
}

TABLE support_tickets {
  id: SERIAL <<PK>>
  user_id: INTEGER NOT NULL <<FK users(id)>>
  message: TEXT NOT NULL
  status: VARCHAR(20) CHECK (status IN ('NEW', 'IN_PROGRESS', 'RESOLVED'))
  created_at: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
}

users ||--o{ plants
users ||--o{ orders
users ||--o{ reviews
users ||--o{ support_tickets
orders ||--o{ order_plants
plants ||--o{ order_plants
orders ||--o| reviews

@enduml