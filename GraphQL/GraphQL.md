
# 📦 GraphQL API Спецификация

## 🧩 Описание

Это техническая спецификация GraphQL API для системы маркетплейса, предназначенная для взаимодействия фронтенда с бэкендом.

Поддерживаются:
- **Запросы (queries)** — для получения данных
- **Мутации (mutations)** — для изменения данных
- **Подписки (subscriptions)** — для получения обновлений в реальном времени

---

## 📚 Типы данных

```graphql
type Seller {
  id: ID!
  companyName: String!
  inn: String!
  email: String!
  phone: String!
  registrationStatus: String!
  createdAt: String!
}

type Product {
  id: ID!
  sellerId: ID!
  name: String!
  description: String
  price: Float!
  sku: String!
  category: String
  status: String!
  createdAt: String!
}

type Inventory {
  id: ID!
  productId: ID!
  quantity: Int!
  warehouseLocation: String!
  updatedAt: String!
}

type Order {
  id: ID!
  productId: ID!
  buyerId: ID!
  quantity: Int!
  status: String!
  orderedAt: String!
}

type Buyer {
  id: ID!
  fullName: String!
  email: String!
  phone: String!
  address: String!
}
```

---

## 🔍 Queries

```graphql
type Query {
  # Sellers
  getSeller(id: ID!): Seller
  listSellers: [Seller]

  # Products
  getProduct(id: ID!): Product
  listProducts(sellerId: ID): [Product]

  # Inventory
  getInventory(productId: ID!): Inventory

  # Orders
  getOrder(id: ID!): Order
  listOrders(buyerId: ID, sellerId: ID): [Order]

  # Buyers
  getBuyer(id: ID!): Buyer
}
```

---

## ✏️ Mutations

```graphql
type Mutation {
  # Seller
  createSeller(companyName: String!, inn: String!, email: String!, phone: String!): Seller
  updateSeller(id: ID!, email: String, phone: String): Seller

  # Product
  createProduct(
    sellerId: ID!,
    name: String!,
    description: String,
    price: Float!,
    sku: String!,
    category: String
  ): Product
  updateProduct(id: ID!, name: String, price: Float, status: String): Product

  # Inventory
  updateInventory(productId: ID!, quantity: Int!, warehouseLocation: String!): Inventory

  # Order
  createOrder(productId: ID!, buyerId: ID!, quantity: Int!): Order
  updateOrderStatus(id: ID!, status: String!): Order

  # Buyer
  createBuyer(fullName: String!, email: String!, phone: String!, address: String!): Buyer
}
```

---

## 📡 Subscriptions

```graphql
type Subscription {
  productCreated(sellerId: ID): Product
  orderCreated(sellerId: ID): Order
  orderStatusUpdated(orderId: ID): Order
}
```

---

## ⚠️ Возможные ошибки

| Код ошибки             | Описание |
|------------------------|----------|
| `SellerNotFound`       | Продавец с указанным ID не найден |
| `ProductNotFound`      | Товар с указанным ID не найден |
| `InventoryMismatch`    | Недостаточно товара на складе |
| `UnauthorizedAccess`   | Нет прав на выполнение действия |
| `ValidationError`      | Ошибка валидации входных данных |
| `InternalServerError`  | Системная ошибка на стороне сервера |

---

## 🚀 Пример использования

```graphql
mutation {
  createProduct(
    sellerId: "abc123",
    name: "Футболка",
    description: "Хлопковая футболка",
    price: 1499.99,
    sku: "TSHIRT-001",
    category: "Одежда"
  ) {
    id
    name
    status
  }
}
```

---

## 📬 Авторы

- Архитектор API: *[Ваше Имя]*
- Поддержка: `support@example.com`

---
