
# 📌 Task: Implement PREFIX Master CRUD + Get By Type API

## 🧾 Objective

Develop a complete **CRUD module** for the `Prefix` master in a Spring Boot project and implement a custom GET API to **fetch prefix by prefixType, companyId, and branchId**.

---

## 🔁 Scope of Work

- Implement the following REST APIs:
  1. Create Prefix
  2. Get All Prefixes
  3. Get Prefix by ID
  4. Update Prefix
  5. Soft Delete Prefix
  6. Get Prefix by prefixType + companyId + branchId

---

## 🧩 Updated Entity Fields in `PrefixVo`

Ensure the following fields exist and are used correctly (without `@SafeHtml`):

- `prefixId` (Long, Primary Key)
- `prefixType` (String)
- `prefix` (String, pattern validated)
- `sequenceNo` (Long)
- `isChangeSequnce` (Integer, default 0)
- `isDeleted` (Integer, default 0)
- `companyId` (Long)
- `branchId` (Long)
- `alterBy` (Long)
- `createdBy` (Long, not updatable)
- `length` (Integer)
- `createdOn` (String, not updatable)
- `modifiedOn` (String)
- `createdbyname` (Transient, UI-only)

---

## 🗂️ Files to Create or Update

- `PrefixVo.java` ✅
- `PrefixRepository.java`
- `PrefixService.java`
- `PrefixServiceImpl.java`
- `PrefixController.java`
- (Optional) `PrefixDto.java`

---

## 🔧 Functional Requirements

### 1. Create Prefix
- **Endpoint:** `POST /api/prefix`
- **Sample Request Body:**
```json
{
  "prefixType": "INVOICE",
  "prefix": "INV-",
  "sequenceNo": 1001,
  "isChangeSequnce": 1,
  "length": 5,
  "companyId": 1,
  "branchId": 1,
  "createdBy": 1,
  "alterBy": 1
}
```

### 2. Get All Prefixes
- **Endpoint:** `GET /api/prefix/all?companyId={companyId}&branchId={branchId}`

### 3. Get Prefix by ID
- **Endpoint:** `GET /api/prefix/{id}`

### 4. Update Prefix
- **Endpoint:** `PUT /api/prefix/{id}`

### 5. Soft Delete Prefix
- **Endpoint:** `DELETE /api/prefix/{id}`

### 6. Get Prefix by Type and companyid and branchid
- **Endpoint:** `GET /api/prefix/{prefixtype}`

- **Repository Method:**
```java
Optional<PrefixVo> findByPrefixTypeAndCompanyIdAndBranchIdAndIsDeleted(
    String prefixType, long companyId, long branchId, int isDeleted);
```

---

## 🧪 Validation Notes

- Regex: `^[a-zA-Z0-9_\s-.,/]+$`
- Use `LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"))` for timestamps.

---

## 📤 Deliverables

- Working CRUD APIs
- Get-by-type API
- Postman collection for all
- Git commit: `"Implemented PREFIX master CRUD and getByType API"`

---

## ⏱️ Timeline

- **Estimated Duration:** 4  hours
- **Deadline:** 15-07-2025

---

## 💡 Notes

- Use mock values for `createdBy`, `alterBy`, `companyId`, `branchId`.
- Skip `createdbyname` in DB ops.
- Always filter with `isDeleted = 0` in read queries.
