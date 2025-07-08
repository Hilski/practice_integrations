# GraphQL API Спецификация

## Это техническая спецификация GraphQL API, предназначенная для взаимодействия фронтенда с бэкендом.

---

## GraphQL Schema (SDL)
```graphql
# Типы данных

type User {
  id: ID!
  name: String!
  email: String!
  orders: [Order!]!
}

type Product {
  id: ID!
  title: String!
  price: Float!
  stock: Int!
}

type Order {
  id: ID!
  user: User!
  products: [Product!]!
  status: OrderStatus!
  createdAt: String!
}

enum OrderStatus {
  PENDING
  COMPLETED
  CANCELLED
}

# Queries

type Query {
  getUser(id: ID!): User
  getAllUsers: [User!]!
  getProduct(id: ID!): Product
  getAllProducts: [Product!]!
  getOrder(id: ID!): Order
  getUserOrders(userId: ID!): [Order!]!
}

# Mutations

type Mutation {
  createUser(name: String!, email: String!): User!
  updateUser(id: ID!, name: String, email: String): User!
  createProduct(title: String!, price: Float!, stock: Int!): Product!
  placeOrder(userId: ID!, productIds: [ID!]!): Order!
  cancelOrder(id: ID!): Order!
}

# Subscriptions

type Subscription {
  userCreated: User!
  orderStatusChanged(orderId: ID!): Order!
  productStockUpdated: Product!
}
```

## Примеры операций
```graphql
# Get user by ID
query GetUser($userId: ID!) {
  getUser(id: $userId) {
    id
    name
    email
  }
}

# List all products
query GetAllProducts {
  getAllProducts {
    id
    title
    price
  }
}

# Create user
mutation CreateUser($name: String!, $email: String!) {
  createUser(name: $name, email: $email) {
    id
    name
  }
}

# Place order
mutation PlaceOrder($userId: ID!, $productIds: [ID!]!) {
  placeOrder(userId: $userId, productIds: $productIds) {
    id
    status
  }
}

# Track order status changes
subscription OnOrderStatusChanged($orderId: ID!) {
  orderStatusChanged(orderId: $orderId) {
    id
    status
  }
}
```
---

## Ошибки

| Код ошибки          | Условие                      | Пример сообщения                  |
|---------------------|------------------------------|-----------------------------------|
| NOT_FOUND           | Ресурс не существует         | Product with ID '456' not found   |
| UNAUTHENTICATED     | Пользователь не авторизован  | Login required                    |
| FORBIDDEN           | Нет прав доступа             | Cannot cancel completed order     |
| BAD_REQUEST         | Невалидные данные            | Email must be a valid address     |
| INTERNAL_ERROR      | Ошибка сервера               | Database connection failed        |

---

## Пример ошибки
```json
# Error
{
  "errors": [
    {
      "message": "User not found",
      "extensions": {
        "code": "NOT_FOUND",
        "details": "User with ID '123' does not exist"
      }
    }
  ]
}
```