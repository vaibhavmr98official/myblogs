
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



---


# 🌍 Location Master API Documentation (Countries, States, Cities)

## 📁 Module: Location Master

---

## 🎯 Objective

Expose APIs to fetch location data for **countries**, **states**, and **cities** using existing tables:  
- `loc_countries` (`CountriesVo`)  
- `loc_state` (`StateVo`)  
- `loc_city` (`CityVo`)

These APIs will be used for dropdowns, forms, and filtering logic throughout the application.

---

## ✅ API Endpoints

### 1. 🔹 Get All Countries

- **Endpoint**: `/api/location/countries`  
- **Method**: `GET`  
- **Description**: Returns all countries.

#### ✅ Sample Response
```json
[
  {
    "countriesId": 1,
    "countriesCode": "IN",
    "countriesName": "India",
    "currencyCode": "INR",
    "currencyName": "Indian Rupee",
    "currencySymbol": "₹",
    "isbrnrequired": 0,
    "countryDialCodePrefix": 91,
    "countryImageName": "india.png"
  }
]
```

---

### 2. 🔹 Get States by Country Code

- **Endpoint**: `/api/location/states/{countryCode}`  
- **Method**: `GET`  
- **Description**: Returns all states under a given country code.

#### 📘 Path Variable
- `countryCode` — ISO code of the country (e.g., `IN`, `US`)

#### ✅ Sample Response
```json
[
  {
    "stateId": 1,
    "stateCode": "GJ",
    "stateName": "Gujarat",
    "countriesCode": "IN",
    "zoneCode": "WEST"
  }
]
```

---

### 3. 🔹 Get Cities by State Code

- **Endpoint**: `/api/location/cities/{stateCode}`  
- **Method**: `GET`  
- **Description**: Returns all cities under a given state.

#### 📘 Path Variable
- `stateCode` — Code of the state (e.g., `GJ`, `MH`)

#### ✅ Sample Response
```json
[
  {
    "cityId": 1,
    "cityCode": "AMD",
    "cityName": "Ahmedabad",
    "stateCode": "GJ",
    "countriesCode": "IN",
    "zoneCode": "WEST"
  }
]
```

---

## 📦 Entity Overview

### 📌 CountriesVo (`loc_countries`)
| Field                | Description                    |
|---------------------|--------------------------------|
| `countriesId`        | Primary key                    |
| `countriesCode`      | ISO country code               |
| `countriesName`      | Full country name              |
| `currencyCode`       | Currency code (e.g., INR, USD) |
| `currencyName`       | Currency name                  |
| `currencySymbol`     | ₹, $, etc.                     |
| `isbrnrequired`      | BRN requirement flag           |
| `countryDialCodePrefix` | Dial code                    |
| `countryImageName`   | Flag or image file name        |

---

### 📌 StateVo (`loc_state`)
| Field                | Description                    |
|---------------------|--------------------------------|
| `stateId`            | Primary key                    |
| `stateCode`          | State code                     |
| `wooCommerceStateCode` | WooCommerce compatible code |
| `stateName`          | State name                     |
| `countriesCode`      | Linked country code            |
| `zoneCode`           | Regional zone                  |

---

### 📌 CityVo (`loc_city`)
| Field                | Description                    |
|---------------------|--------------------------------|
| `cityId`             | Primary key                    |
| `cityCode`           | City code                      |
| `cityName`           | City name                      |
| `stateCode`          | Linked state code              |
| `countriesCode`      | Linked country code            |
| `zoneCode`           | Regional zone                  |

---

## 📌 Controller Location

All APIs will reside in:  
`LocationController.java` with base path: `/api/location`

---

## 🧪 Testing Checklist

- [ ] Test with country codes: `IN`, `US`, etc.
- [ ] Validate response structure and data mapping.
- [ ] Ensure relationship mapping: Country → State → City.

---

> ✅ This module will be reused across user onboarding, address forms, reporting filters, and business settings.
