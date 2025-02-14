# CQRS (Command Query Responsibility Segregation)

## Introduction
CQRS (Command Query Responsibility Segregation) is a software architectural pattern that separates read and write operations for better scalability, flexibility, and maintainability.

## Why CQRS?
- **Scalability**: Read and write operations can be scaled independently.
- **Performance**: Queries can be optimized separately from commands.
- **Security**: Write and read permissions can be managed independently.
- **Flexibility**: Different storage technologies can be used for reads and writes.
- **Event Sourcing**: Captures state changes as a sequence of events.

## Architecture Overview
CQRS separates the system into two distinct models:
1. **Command Side**: Handles write operations and business logic.
2. **Query Side**: Handles read operations optimized for fast access.

### Diagram
```
   +------------+       +------------+
   |  Command   |       |    Query   |
   |   Model    |       |    Model   |
   +------------+       +------------+
         |                   |
 +----------------+  +----------------+
 |  Event Store  |  | Read Database  |
 +----------------+  +----------------+
```

## Use Cases
### 1. **Banking System**
- **Commands**: Create Account, Deposit Money, Withdraw Money
- **Queries**: Fetch Account Balance, Transaction History

### 2. **E-Commerce System**
- **Commands**: Place Order, Cancel Order, Update Inventory
- **Queries**: Get Product Details, Order Status, Customer Purchases

### 3. **Messaging System**
- **Commands**: Send Message, Delete Message
- **Queries**: Fetch Messages, Search Conversations

## Key Components
1. **Command Handlers**: Process and validate write requests.
2. **Event Store**: Stores state-changing events.
3. **Event Handlers**: Listens to events and updates read models.
4. **Read Models (Projections)**: Optimized database for querying.

## Implementation Details
### **1️⃣ `AccountAggregate.java` (Aggregate Root)**
📌 **Purpose:**
- Manages business logic for account operations.
- Uses **Event Sourcing** to apply state changes.

📌 **Key Features:**
- **Handles Commands**: `CreateAccountCommand`, `DepositMoneyCommand`, `WithdrawMoneyCommand`.
- **Applies Events**: `AccountCreatedEvent`, `MoneyDepositedEvent`, `MoneyWithdrawnEvent`.
- **Maintains State**: `accountId` and `balance`.

🔹 **Important Methods:**
```java
@CommandHandler
public AccountAggregate(CreateAccountCommand command) {
    apply(new AccountCreatedEvent(command.accountId(), command.initialBalance()));
}
```

### **2️⃣ Event Definitions**
📌 **Purpose:**
- Events represent state changes in the system.

📌 **Key Events:**
- **`AccountCreatedEvent.java`**: Event triggered when an account is created.
- **`MoneyDepositedEvent.java`**: Event triggered when money is deposited.
- **`MoneyWithdrawnEvent.java`**: Event triggered when money is withdrawn.

🔹 **Example:**
```java
public record AccountCreatedEvent(String accountId, BigDecimal balance) {}
```

### **3️⃣ Event Handlers (`AccountEventHandler.java`)**
📌 **Purpose:**
- Listens to events and updates the **read model** (`AccountView`).

🔹 **Important Methods:**
```java
@EventHandler
public void on(AccountCreatedEvent event) {
    repository.save(new AccountView(event.accountId(), event.balance()));
}
```

### **4️⃣ Query Side (`AccountQueryController.java`)**
📌 **Purpose:**
- Handles read operations efficiently.

🔹 **Endpoints:**
```java
@GetMapping("/{id}")
public AccountView getAccount(@PathVariable String id) {
    return repository.findById(id).orElseThrow();
}
```

### **5️⃣ Repository (`AccountViewRepository.java`)**
📌 **Purpose:**
- Provides an interface for querying accounts stored in the **read database**.

🔹 **Implementation:**
```java
public interface AccountViewRepository extends JpaRepository<AccountView, String> {}
```

### **6️⃣ Read Model (`AccountView.java`)**
📌 **Purpose:**
- Represents the **projection** of an account in the read database.

🔹 **Implementation:**
```java
@Entity
public class AccountView {
    @Id
    private String accountId;
    private BigDecimal balance;
}
```

## Useful Links
- [Microsoft CQRS Guide](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)
- [Martin Fowler on CQRS](https://martinfowler.com/bliki/CQRS.html)
- [Event Sourcing Explained](https://eventstore.com/event-sourcing)

## Conclusion
CQRS is a powerful architectural pattern that enhances scalability and flexibility, especially in complex systems that require high performance and reliability. By separating reads and writes, it enables better optimizations and independent scaling.

