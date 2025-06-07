# ✅ Technical Architecture Document: Invoom Admin Panel Masters

**Project Title:** Invoom Admin Panel – Masters
**Prepared By:** Vaibhav Rawat [ T7 Solutions ]
**Date:** June 7, 2025

---

## 1. 🌟 Objective

To develop a multi-tenant SaaS admin panel where each client is treated as a separate tenant. The platform enables internal admins to manage clients, plans, subscriptions, and features via a central master DB while isolating tenant data in their own databases.

---

## 2. 🧩 Core CRUD Modules and Field Mappings

### 2.1 Client Master

| Field        | Description         |
| ------------ | ------------------- |
| client\_id   | Unique ID           |
| client\_name | Client Name         |
| email        | Email Address       |
| mobile       | Mobile Number       |
| signup\_type | Type of Signup      |
| country      | Country             |
| status       | Status              |
| plan\_id     | Linked Plan ID      |
| db\_name     | Tenant DB Name      |
| tags         | Client Tags (Array) |
| created\_on  | Created Timestamp   |

**Sample:**

```json
{
  "clientName": "Innovate Tech",
  "email": "admin@innovate.com",
  "mobile": "+91-9876543210",
  "signupType": "Business",
  "country": "India",
  "status": "Active",
  "planId": 2,
  "dbName": "tenant_innovate",
  "tags": ["Beta", "Premium"],
  "createdOn": "2025-06-01T10:00:00"
}
```

---

### 2.2 Plan Master

| Field                   | Description                  |
| ----------------------- | ---------------------------- |
| plan\_id                | Unique Plan ID               |
| plan\_name              | Name of the Plan             |
| plan\_type              | Business/Individual          |
| default\_currency       | Default Currency (e.g., INR) |
| default\_monthly\_price | Default Monthly Price        |
| default\_annual\_price  | Default Annual Price         |
| created\_by             | Admin User Creating Plan     |
| created\_on             | Plan Creation Timestamp      |

#### 2.2.1 Plan Limit

| Field        | Description        |
| ------------ | ------------------ |
| limit\_type  | Type of Limit      |
| limit\_value | Value of the Limit |

#### 2.2.2 Country Pricing

| Field          | Description         |
| -------------- | ------------------- |
| region         | Geographical Region |
| country        | Country Name        |
| currency       | Currency Code       |
| monthly\_price | Monthly Price       |
| annual\_price  | Annual Price        |

**Full Sample JSON:**

```json
{
  "planId": 2,
  "planName": "Business Growth",
  "planType": "Business",
  "defaultCurrency": "INR",
  "defaultMonthlyPrice": 999.99,
  "defaultAnnualPrice": 9999.99,
  "createdBy": "admin",
  "createdOn": "2025-06-01T09:00:00",
  "limits": [
    {
      "limitType": "users",
      "limitValue": 100
    },
    {
      "limitType": "documents",
      "limitValue": 1000
    }
  ],
  "countryPricing": [
    {
      "region": "Europe",
      "country": "UK",
      "currency": "GBP",
      "monthlyPrice": 90.0,
      "annualPrice": 950.0
    },
    {
      "region": "Asia",
      "country": "India",
      "currency": "INR",
      "monthlyPrice": 999.99,
      "annualPrice": 9999.99
    }
  ]
}
```

---

### 2.3 Feature Master

| Field         | Description         |
| ------------- | ------------------- |
| feature\_id   | Unique Feature ID   |
| feature\_name | Name of the Feature |
| description   | Feature Description |

**Sample:**

```json
{
  "featureName": "Auto-Publish",
  "description": "Automatically publish documents after processing."
}
```

---

### 2.4 PlanFeature Mapping

| Field       | Description |
| ----------- | ----------- |
| plan\_id    | Plan ID     |
| feature\_id | Feature ID  |

**Sample:**

```json
{
  "planId": 1,
  "featureId": 3
}
```

---

### 2.5 Subscription Master

| Field                | Description             |
| -------------------- | ----------------------- |
| subscription\_id     | Subscription ID         |
| client\_id           | Linked Client ID        |
| plan\_id             | Linked Plan ID          |
| start\_date          | Start Date              |
| renewal\_date        | Renewal Date            |
| billing\_cycle       | Monthly/Annual          |
| auto\_renew          | Auto Renewal Enabled    |
| status               | Active/Inactive         |
| documents\_used      | Documents Consumed      |
| users\_used          | Users Consumed          |
| clients\_used        | Nested Clients (if any) |
| cancellation\_reason | Cancellation Reason     |
| payment\_method      | Credit Card/UPI/etc.    |
| created\_on          | Created Timestamp       |

**Sample:**

```json
{
  "clientId": 1,
  "planId": 2,
  "startDate": "2025-06-01",
  "renewalDate": "2026-06-01",
  "billingCycle": "Annual",
  "autoRenew": true,
  "status": "Active",
  "documentsUsed": 150,
  "usersUsed": 10,
  "clientsUsed": 1,
  "paymentMethod": "Credit Card",
  "createdOn": "2025-06-01T10:00:00"
}
```

---

### 2.6 System Activity Log

| Field            | Description                      |
| ---------------- | -------------------------------- |
| log\_id          | Log ID                           |
| user\_id         | User ID                          |
| role\_type       | ADMIN/USER/etc.                  |
| action\_type     | LOGIN/LOGOUT/UPDATE/etc.         |
| ip\_address      | User IP                          |
| location         | City, Country                    |
| timestamp        | Action Time                      |
| additional\_info | Device/OS/Browser Details (JSON) |

**Sample:**

```json
{
  "userId": 5,
  "roleType": "ADMIN",
  "actionType": "LOGIN",
  "ipAddress": "192.168.1.12",
  "location": "Mumbai, India",
  "timestamp": "2025-06-07T08:00:00Z",
  "additionalInfo": {
    "device": "Chrome",
    "os": "Windows 11",
    "browserVersion": "123.0.0.1"
  }
}
```

---

## 3. 🧱 PostgreSQL Schema Overview

* Normalize limits in `plan_limit`
* Support regional pricing in `country_pricing`
* Add generic `system_activity_log` table

---

## 4. ⚙️ Spring Boot Project Structure

```
src/
├── controller/
│   └── [module]/[Module]Controller.java
├── service/
│   └── [module]/[Module]Service.java
│   └── [module]/[Module]ServiceImpl.java
├── repository/
│   └── [module]/[Module]Repository.java
├── dto/
│   └── [module]/[Module]Dto.java
├── vo/
│   └── [module]/[Module]Vo.java
```

---

## 5. 🌿 Git Branch Naming Convention

| Module              | Package Prefix | Git Branch Name           |
| ------------------- | -------------- | ------------------------- |
| Client              | client         | feature/client-crud       |
| Plan                | plan           | feature/plan-crud         |
| Plan Limit          | planlimit      | feature/planlimit-crud    |
| Country Pricing     | countrypricing | feature/country-pricing   |
| Feature             | feature        | feature/feature-crud      |
| PlanFeature Mapping | planfeature    | feature/planfeature-map   |
| Subscription        | subscription   | feature/subscription-crud |
| System Activity Log | systemlog      | feature/system-logs       |

---

## 6. 🔗 JPA Entity Mapping Summary

* `Plan` → `PlanLimit`: `@OneToMany(mappedBy = "plan")`
* `Plan` → `CountryPricing`: `@OneToMany(mappedBy = "plan")`
* `Client` → `Subscription`: `@OneToMany`
* `Plan` ↔ `Feature`: `@ManyToMany`

---