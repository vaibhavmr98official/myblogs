
# 🧩 Task: Implement CRUD for Limit Master Module

## 📦 Entity: `limit_master`

| Column      | Type      | Description                     |
|-------------|-----------|---------------------------------|
| limit_id    | BIGINT PK | Auto-generated primary key      |
| limit_name  | VARCHAR   | e.g., Limit Clients, Limit Users|
| limit_type  | VARCHAR   | Optional type classification    |

---

## 🔧 Task Objective

Implement full CRUD (Create, Read, Update, Delete) functionality for the `limit_master` table in a Spring Boot application.

---

## 🗂️ Files to Create/Update

- `LimitMasterVo.java` (Entity)
- `LimitMasterRepository.java` (Repository)
- `LimitMasterService.java` (Service Interface)
- `LimitMasterServiceImpl.java` (Service Implementation)
- `LimitMasterController.java` (REST Controller)
- (Optional) `LimitMasterDto.java` (DTO Layer)

---

## 🔧 Functional Requirements

### ✅ 1. Create Limit
- **Endpoint:** `POST /api/limits`
- **Request Body:**
```json
{
  "limitName": "Limit Clients",
  "limitType": "numeric"
}
```

---

### ✅ 2. Get All Limits
- **Endpoint:** `GET /api/limits`
- **Description:** Fetch all active limits.

---

### ✅ 3. Get Limit by ID
- **Endpoint:** `GET /api/limits/{id}`
- **Description:** Fetch a specific limit using its ID.

---

### ✅ 4. Update Limit
- **Endpoint:** `PUT /api/limits/{id}`
- **Request Body:**
```json
{
  "limitName": "Limit Users",
  "limitType": "numeric"
}
```

---

### ✅ 5. Delete Limit (Soft Delete or Hard Delete)
- **Endpoint:** `DELETE /api/limits/{id}`

> Optional: Implement soft delete by adding `isDeleted` column if needed.

---

## 🔐 Validation Rules

- `limitName` must be unique (enforce in DB or service layer).
- `limitName` should not be null or blank.
- `limitType` is optional but can be used for grouping.

---

## 🧪 Deliverables

- Fully working CRUD endpoints.
- Swagger/Postman test collection.
- Git commit with message: `"Implemented CRUD for Limit Master"`

---

## ⏱️ Estimated Time

- **Time:** 3–4 hours

---

## 💡 Notes

- Ensure proper error messages (e.g., 404 for not found).
- Add `@CrossOrigin` if frontend needs access.
- Use `@RestController` and standard Spring conventions.
