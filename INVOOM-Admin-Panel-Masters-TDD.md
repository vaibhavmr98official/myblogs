
**Technical Architecture Document: Invoom Master**

---

**Project Title:** Invoom Admin Panel

**Created By:** T7 Solutions

**Prepared For:** Developer Team

**Date:** June 7, 2025

---

## 1. Objective

To develop a multi-tenant SaaS admin panel where each client is treated as a separate tenant. The platform allows internal admins to manage clients, plans, and subscriptions from a centralized master database. Each client will have its own isolated PostgreSQL database.

---

## 2. Core CRUD Modules and Mappings

### 2.1 Client Master
**Purpose:** Registers clients/tenants.

**Fields:**
- client_id (PK)
- client_name
- email (unique)
- mobile
- signup_type ('Practice' / 'Business')
- country
- status ('Active', 'Suspended', 'Inactive')
- plan_id (FK to plan)
- db_name (for tenant DB routing)
- tags (TEXT[])
- created_on

**Entity Relationship:**
- One Client has One Plan
- One Client can have Many Subscriptions

### 2.2 Plan Master
**Purpose:** Define reusable subscription packages.

**Fields:**
- plan_id (PK)
- plan_name
- plan_type ('Practice' / 'Business')
- default_currency
- default_monthly_price
- default_annual_price
- limits: clients, users, documents, bank_statements, line_items, supplier_statements
- created_by
- created_on

**Entity Relationship:**
- One Plan has Many Features (via plan_feature)
- One Plan can be assigned to Many Clients

### 2.3 Feature Master
**Purpose:** List and manage platform features.

**Fields:**
- feature_id (PK)
- feature_name
- description

### 2.4 PlanFeature Mapping (Many-to-Many)
**Fields:**
- plan_id (FK to plan)
- feature_id (FK to feature)

**Entity Relationship:**
- Many Plans <--> Many Features

### 2.5 Country Pricing Table
**Purpose:** Country-wise override for default plan pricing.

**Fields:**
- pricing_id (PK)
- plan_id (FK to plan)
- country
- currency
- monthly_price
- annual_price

### 2.6 Subscription Master
**Purpose:** Tracks client subscriptions to plans.

**Fields:**
- subscription_id (PK)
- client_id (FK to client)
- plan_id (FK to plan)
- start_date
- renewal_date
- billing_cycle ('Monthly' / 'Annual')
- auto_renew (boolean)
- status ('Active', 'Cancelled', 'RenewalDue')
- documents_used
- users_used
- clients_used
- cancellation_reason
- payment_method
- created_on

**Entity Relationship:**
- One Client can have Many Subscriptions
- One Subscription is tied to One Plan

---

## 3. PostgreSQL Schema Overview

Refer to Section 2 for field-level schema.
Each table should be normalized, with proper foreign key constraints and indexing on frequently searched fields (email, db_name, plan_id, etc).

---

## 4. Spring Boot Mappings (JPA)

**Entity Classes to Create:**
- Client
- Plan
- Feature
- PlanFeature (Join Table)
- CountryPricing
- Subscription

**Relationships:**
- Client → Plan: `@ManyToOne`
- Client → Subscription: `@OneToMany`
- Plan → Feature: `@ManyToMany` via PlanFeature
- Plan → CountryPricing: `@OneToMany`
- Subscription → Client, Plan: `@ManyToOne`

Use DTOs and services for each CRUD module.

---

## 5. Deliverables
- Create PostgreSQL schema with the above tables.
- Build Spring Boot JPA entity models.
- Build full CRUD services and APIs for:
  - Client
  - Plan
  - Feature
  - Subscription
- Ensure referential integrity and cascading behavior where necessary.
- Document Swagger/OpenAPI endpoints.

[//]: # (---)

[//]: # (## 6. Next Steps)

[//]: # (- Complete database migration scripts.)

[//]: # (- Set up initial dummy data for testing.)

[//]: # (- Implement unit/integration tests.)

---

[//]: # (**Document Ends**)
