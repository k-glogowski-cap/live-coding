# Senior Python Developer Live Coding Exercise

## Overview

You are implementing a small Purchase Order Management service for an internal procurement team.

The goal of this exercise is to assess your ability to:

- Design a relational database schema from business requirements
- Model entities and relationships
- Implement business logic
- Build REST APIs
- Write tests
- Discuss scalability and architecture trade-offs

---

# Business Domain

The company runs an internal procurement team that buys goods from a number of external suppliers. Each supplier offers a range of products, and the same product is often available from several suppliers at the same time. Because every supplier sets its own price, the price of a product depends on who it is bought from – a box of printer paper may cost one amount at one supplier and a different amount at another.

Purchases are initiated by employees. Every employee works in exactly one department, and a department usually has many employees. Some employees act as managers, which gives them the authority to approve purchases made in the company.

When an employee needs to buy something, they prepare a purchase order addressed to a single supplier. An order lists one or more items to be bought; for each item it records which product is ordered, in what quantity, and at what unit price. The unit price on the order is the price agreed with the supplier for that particular order.

A purchase order goes through a simple workflow. It starts as a draft while the employee is still preparing it. Once complete, it is submitted for approval. A manager then reviews it and either approves or rejects it – ordinary employees are not allowed to approve orders. After an approved order has been fulfilled, it is marked as delivered. Suppliers do not always deliver exactly what was ordered, so the quantity actually delivered may differ from the quantity on the order and needs to be tracked separately. In the system, these stages are represented by the statuses `DRAFT`, `SUBMITTED`, `APPROVED`, `REJECTED` and `DELIVERED`.

For audit purposes, the company must be able to reconstruct the full history of every order: each change of status, when it happened, and who made it. Simply overwriting the current status is not sufficient.

Finally, the procurement team plans to handle supplier invoices in this system in the future, so the solution should be designed in a way that makes adding invoicing straightforward.

---




# Part 1 – Database Design (20 minutes)

Design a relational database schema based on the business domain described above.

## Tasks

1. Identify all entities.
2. Define relationships.
3. Define primary keys.
4. Define foreign keys.
5. Explain normalization decisions.

## Deliverables

- ERD diagram (whiteboard or draw.io)
- Schema description

---

# Part 2 – SQLAlchemy Models (20 minutes)

Implement SQLAlchemy models for the designed schema.

## Expectations

Use SQLAlchemy 2.x style.

### Example

```python
class Supplier(Base):
    __tablename__ = "suppliers"

    id = mapped_column(Integer, primary_key=True)
    name = mapped_column(String(255), nullable=False)
```

---

# Part 3 – Submit Purchase Order (30 minutes)

Implement the following business operation:

```python
def submit_purchase_order(
    order_id: int,
    employee_id: int,
) -> PurchaseOrder:
    ...
```

## Requirements

- Order must exist.
- Order must contain at least one order line.
- Current status must be DRAFT.
- Status changes to SUBMITTED.
- Status history must be recorded.

---

# Part 4 – Approve Purchase Order (20 minutes)

Implement approval workflow.

```python
def approve_purchase_order(
    order_id: int,
    manager_id: int,
) -> PurchaseOrder:
    ...
```

## Requirements

- Order must exist.
- Employee must exist.
- Employee must have manager privileges.
- Order must currently be SUBMITTED.
- Status changes to APPROVED.
- Status history must be recorded.
- Approver information must be stored.

---

# Part 5 – FastAPI Endpoint (20 minutes)

Create an endpoint that approves a purchase order.

## Endpoint

```http
POST /purchase-orders/{id}/approve
```

## Request

```json
{
  "manager_id": 5
}
```

## Response

```json
{
  "id": 123,
  "status": "APPROVED"
}
```

---

# Part 6 – Reporting Query (15 minutes)

Provide a query that returns:

> Top 10 suppliers by total approved purchase order value.

## Expected Output

```text
supplier_name
total_amount
number_of_orders
```

The solution can be written using:

- SQLAlchemy ORM

---

# Part 7 – Unit Tests (15 minutes)

Write unit tests for:

```python
approve_purchase_order()
```

---

# Optional Stack

You may assume the following technology stack:

- Python 3.12+
- FastAPI
- SQLAlchemy 2.x
- PostgreSQL
- Pytest

The focus is on reasoning and design quality rather than complete implementation.

