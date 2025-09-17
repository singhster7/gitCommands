          ┌───────────────┐
          │   Postman /   │  (Without Kafka)
          │   Client App  │
          └───────┬───────┘
                  │  (REST API calls)
                  ▼
         ┌─────────────────────┐
         │   OrderController   │
         │  (Spring Boot REST) │
         └─────────┬───────────┘
                   │
                   ▼
         ┌─────────────────────┐
         │    OrderService     │
         │  (Business Logic)   │
         └───────┬───────┬─────┘
                 │       │
   ┌─────────────┘       └─────────────┐
   ▼                                   ▼
┌──────────────┐                ┌──────────────┐
│ Pricing API  │                │ Inventory API│
│ (External)   │                │ (External)   │
└───────┬──────┘                └───────┬──────┘
        │                               │
        ▼                               ▼
   (unit price)                     (reserve stock)


                 ┌─────────────────────┐
                 │  OrderRepository    │
                 │ (Spring Data JPA)   │
                 └─────────┬───────────┘
                           │
                           ▼
                 ┌─────────────────────┐
                 │   PostgreSQL DB     │
                 │ (orders table)      │
                 └─────────────────────┘

      Implementation with Kafka.
          ┌───────────────┐
          │   Postman /   │
          │   Client App  │
          └───────┬───────┘
                  │ (REST API calls)
                  ▼
         ┌─────────────────────┐
         │   OrderController   │
         │  (Spring Boot REST) │
         └─────────┬───────────┘
                   │
                   ▼
         ┌─────────────────────┐
         │    OrderService     │
         │  (Business Logic)   │
         └───────┬─────┬───────┘
                 │     │
                 │     │ Publishes events
                 │     ▼
                 │   ┌───────────────────┐
                 │   │   Kafka Topic     │
                 │   │ (order.created /  │
                 │   │  order.failed)    │
                 │   └───────┬───────────┘
                 │           │
   ┌─────────────┘           ▼
   ▼                   ┌───────────────┐
┌──────────────┐       │ Inventory     │
│ Pricing API  │       │ Service       │
│ (External)   │       │ (consumer of  │
└──────────────┘       │ inventory.updated) 
                        └───────┬───────┘
                                │
                                ▼
                         ┌───────────────┐
                         │ OrderService  │
                         │ (updates DB   │
                         │ if stock low) │
                         └───────────────┘

                 ┌─────────────────────┐
                 │  OrderRepository    │
                 │ (Spring Data JPA)   │
                 └─────────┬───────────┘
                           │
                           ▼
                 ┌─────────────────────┐
                 │   PostgreSQL DB     │
                 │ (orders table)      │
                 └─────────────────────┘

