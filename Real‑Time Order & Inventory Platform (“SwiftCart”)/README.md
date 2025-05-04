## **Real‑Time Order & Inventory Platform (“SwiftCart”)**


With this project we are going to dive into **event‑driven micro‑services, caching, and real‑time processing** with SpringBoot, Kafka, and Redis.

---

### 1. Business Problem—why this matters

“SwiftCart” is a lightweight e‑commerce backbone that must:

1. **Accept customer orders** from a public REST gateway.
2. **Reserve and decrement stock** in real‑time so users never buy an out‑of‑stock item.
3. **Emit events** (order‑created, stock‑reserved, payment‑approved, shipment‑dispatched) so downstream teams—payments, shipping, analytics—stay in sync.
4. **Expose near–instant product availability** to the website and mobile apps.
5. **Recover gracefully** if one component goes down, with zero data loss.

---

### 2. High‑Level Architecture

| Micro‑service               | Tech focus                                                  | Key responsibilities                                                                                                                                              |
| --------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **API‑Gateway**             | Spring Cloud Gateway                                        | Auth, request validation, routing                                                                                                                                 |
| **Order‑Service**           | SpringBoot + JPA + Kafka producer                          | REST `POST /orders`, write order to DB, publish `ORDER_CREATED` to Kafka                                                                                          |
| **Inventory‑Service**       | SpringBoot + Kafka consumer/producer, Redis              | Subscribe to `ORDER_CREATED`, check & reserve stock atomically using **Redis** (fast in‑memory counter + Lua script). Publish `STOCK_RESERVED` or `STOCK_FAILED`. |
| **Payment‑Service**         | SpringBoot, external mock payment, Kafka consumer/producer | Listen for `STOCK_RESERVED`, call fake payment provider, emit `PAYMENT_APPROVED` or `PAYMENT_FAILED`.                                                             |
| **Shipping‑Service**        | SpringBoot, Kafka consumer                                 | Start shipment on `PAYMENT_APPROVED`, update DB, emit `SHIPMENT_DISPATCHED`.                                                                                      |
| **Notification‑Service**    | SpringBoot, Kafka consumer, Email/SMS mock                 | Send e‑mails/SMS for each order event.                                                                                                                            |
| **Real‑time Product Cache** | Redis                                                       | Hold product details & current stock qty—updated by Inventory‑Service, queried by API‑Gateway (`/products/{id}`).                                                 |
| **Analytics‑Sink** (bonus)  | Kafka Streams or ksqlDB                                     | Build running dashboard of daily order count, revenue.                                                                                                            |

*All inter‑service communication after the gateway is **asynchronous Kafka topics**; services remain loosely coupled.*

---

### 3. Core Requirements

1. **Event Schema**
   *Use JSON + a version field.* Example `OrderCreatedEvent`

   ```json
   { "version":1, "orderId":"...", "userId":"...", "items":[{"sku":"A1","qty":2}], "ts":"2025‑05‑04T12:00:00Z" }
   ```

   Document each event type in a shared `docs/events` folder.

2. **Exactly‑once stock reservation**
   *Store product stock in Redis hashes.* Use a Lua script to `DECRBY` atomically and return success/fail to avoid race conditions.

3. **Outbox Pattern (reliability)**
   In Order‑Service and Inventory‑Service persist the event in the same DB txn (Spring Boot + JPA) then publish to Kafka via Debezium or manual polling to guarantee no lost messages.

4. **Idempotency**
   Every event carries a UUID; consumers track processed IDs in Redis SET with TTL to ensure repeat deliveries are ignored safely.

5. **Graceful failure & retries**
   Use **Kafka dead‑letter topics**; failed payments or stock deficits flow to a compensating saga that triggers `ORDER_CANCELLED`.

6. **Caching layer**
   Product reads go to Redis first; fallback to relational DB; use **cache‑aside** pattern with TTL invalidation when stock changes.

7. **Observability**

    * Centralised logging with Spring Sleuth + OpenTelemetry.
    * Health endpoints (`/actuator/health`), Prometheus metrics, Grafana dashboard.

---

### 4. Stretch Goals

| Feature                                                              | Why it’s useful                  |
| -------------------------------------------------------------------- | -------------------------------- |
| **Kafka Streams aggregation** for hourly sales totals                | Real‑time analytics experience   |
| **Docker‑Compose** for local cluster (Kafka, Redis, MySQL, Grafana)  | DevOps familiarity               |
| **Kubernetes manifests** (optional)                                  | Cloud‑native deployment practice |
| **Circuit‑breaker / retry** with Resilience4j on Payment integration | Fault tolerance skills           |
| **Security**: OAuth2 / JWT at API‑Gateway                            | End‑to‑end authentication        |

---

### 5. Learning Outcomes

* **Event‑Driven Design**: design, version, and evolve Kafka topics.
* **Transactional Messaging**: implement the Outbox pattern & saga compensation.
* **Redis Mastery**: use it for both caching and atomic operations.
* **Resilient Micro‑services**: retries, DLQ, idempotency.
* **End‑to‑End Delivery**: from REST request to async workflow to final user notification.

---

### 6. Suggested Milestones (8–10weeks)

1. **Week 1‑2** – Project bootstrap, API‑Gateway + Order‑Service CRUD.
2. **Week 3‑4** – Kafka local cluster, event contracts, Inventory‑Service with Redis atomic stock.
3. **Week 5** – Payment‑Service mock + saga happy path.
4. **Week 6** – Shipping‑Service & Notification‑Service.
5. **Week 7** – Error paths, dead‑letter topics, order cancellation flow.
6. **Week 8** – Caching layer, load tests with Gatling.
7. **Week 9** – Observability stack, dashboards.
8. **Week 10** – Stretch goals (Streams analytics, Docker‑compose polish, README).

---

### 7. What to Deliver

* **Source code** split by service.
* **Docker‑compose** to spin up everything locally.
* **Postman collection** or Swagger docs for gateway endpoints.
* **Architecture README** with diagrams (C4 level 2 + event flow sequence).
* **Demo script / video**: place an order, watch events flow, check Redis, see metrics.

---

This project mirrors real enterprise systems and will let you **talk confidently about Kafka, Redis, micro‑service patterns, and resilience** in any SpringBoot interview.
