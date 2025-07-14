
# 📝 Task: Currency Master CRUD

## 📁 Module: Currency Master

---

## 🎯 Objective

Create a **Currency Master module** that allows performing full **CRUD operations** (Create, Read, Update, Delete) on currency data
Each currency will also include the associated **`countryImageName`**, which will be used to display country flags or images in the UI.

---

## ✅ Key Notes

**Fields to Manage (from `loc_countries`):**

- `currencyCode`
- `currencyName`
- `currencySymbol`
- `countriesCode`
- `countryImageName`

> 🔸 Operations will be implemented in a **separate controller** named `CurrencyController`.  

---

## 🛠️ Task Breakdown

### 1. 📦 Create DTOs

#### 🔹 `CurrencyRequestDTO` – for create/update requests

```java
@Data
public class CurrencyRequestDTO {
    private String currencyCode;
    private String currencyName;
    private String currencySymbol;
    private String countriesCode;
    private String countryImageName;
}
```

#### 🔹 `CurrencyResponseDTO` – for list/detail responses

```java
@Data
public class CurrencyResponseDTO {
    private Long countriesId;
    private String currencyCode;
    private String currencyName;
    private String currencySymbol;
    private String countriesCode;
    private String countryImageName;
}
```

---

### 2. 📂 Create Controller: `CurrencyController`

#### 📌 Endpoints to Implement

| Method | Endpoint                            | Action                     |
|--------|-------------------------------------|----------------------------|
| GET    | `/api/location/currencies`          | List all currencies        |
| GET    | `/api/location/currencies/{id}`     | Get currency by ID         |
| POST   | `/api/location/currencies`          | Add new currency           |
| PUT    | `/api/location/currencies/{id}`     | Update existing currency   |
| DELETE | `/api/location/currencies/{id}`     | Delete currency by ID      |

---

### 3. 🧠 Service Layer

Create a `CurrencyService` interface and implementation (`CurrencyServiceImpl`) to handle business logic:

```java
List<CurrencyResponseDTO> getAllCurrencies();
CurrencyResponseDTO getCurrencyById(Long id);
CurrencyResponseDTO createCurrency(CurrencyRequestDTO dto);
CurrencyResponseDTO updateCurrency(Long id, CurrencyRequestDTO dto);
void deleteCurrency(Long id);
```

---

### 4. 📇 Repository

Create a dedicated repository:
- `CurrencyRepository` (can extend `JpaRepository<CountriesVo, Long>`)

Add custom methods if needed, such as:
```java
Optional<CountriesVo> findByCurrencyCode(String currencyCode);
```

---

### 5. 🧪 Testing

- Add basic field validation (e.g., `@NotNull`, `@NotBlank`)
- Use **Swagger** or **Postman** to test all endpoints.
- Confirm frontend displays flags correctly using the `countryImageName`.

---

## 🔍 Validation Rules

- `currencyCode`, `currencyName`, and `currencySymbol` are **mandatory**
- Use `countryImageName` only for frontend image rendering (do not validate file existence)
- Prevent duplicate `currencyCode` entries during create/update operations

---

> ✅ Final code should be committed to:  
> `feature/currency-master-crud`  
> Ensure clean formatting, Swagger integration, and exception handling are added.
