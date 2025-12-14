# Deep Conceptual Explanation: MuleSoft RAML API Design – SwiftCourier

---

## 1. API (Application Programming Interface)

### What an API really is
An API is a **contract** that defines how two software systems communicate.  

**Analogy:**
- A restaurant menu is an API  
- The kitchen is the backend system  
- The customer is the frontend or another system  

**The Menu:**
- Lists what you can request  
- Tells how to request it  
- Tells what you will receive  

**Important:**  
An API does **not** explain how the food is cooked. It only explains **what is available and how to order it**. That is exactly what your RAML file does.

---

## 2. REST (Representational State Transfer)

### What REST really means
REST is an **architectural style**, not a programming language.  
It says:  
> "Design APIs using real-world resources and standard rules."

### Core REST Ideas

#### 2.1 Resources
- A **resource** is a **thing**, not an action.  
- Examples: Customer, Parcel, Driver, Payment  

**Bad design:** `/createCustomer`  
**Good REST design:** `/customers`  

#### 2.2 HTTP Methods (Verbs)
REST separates:
- **WHAT** you act on → Resource (noun)  
- **HOW** you act → HTTP method (verb)

| Method | Meaning |
|--------|---------|
| GET    | Read data (no change) |
| POST   | Create new data |
| PUT    | Replace entire resource |
| PATCH  | Update part of a resource |
| DELETE | Remove resource |

**Benefits:** Predictable, Scalable, Easy to learn  

#### 2.3 Statelessness
- Every request must contain **all required information**.  
- Server does **not remember previous requests**.  

**Why this matters:** Better performance, Easier scaling, Safer systems  

---

## 3. RAML (RESTful API Modeling Language)

### What RAML really is
- A **design language**  
- A **specification**  
- A **blueprint**  

**It is NOT executable code.**  

**RAML answers:**
- What endpoints exist?  
- What data is accepted?  
- What data is returned?  
- What errors are possible?  

### Why RAML uses YAML
YAML is:  
- Human-readable  
- Indentation-based  
- Clean and minimal  

**Benefit:** Developers, architects, and non-programmers can understand API design easily.

---

## 4. API-First Design

### Traditional Approach (BAD)
1. Write backend code  
2. Expose endpoints later  
3. Frontend waits  
4. Many misunderstandings  

### API-First Approach (GOOD)
1. Design API contract (RAML)  
2. Everyone agrees on it  
3. Backend & frontend work in parallel  
4. Fewer bugs and conflicts  

**Why companies love RAML:** Supports API-first development.

---

## 5. API Metadata

**Metadata = data about data**  

### Key Concepts
- **Versioning (v1)**  
  - APIs evolve  
  - Old clients must not break  
  - Example: `/v1/customers`, `/v2/customers`  

- **Media Type (application/json)**  
  - Lightweight  
  - Human-readable  
  - Industry standard  

---

## 6. Resources and Endpoints

**Resource vs Endpoint:**  
- Resource → Conceptual entity (e.g., Customer)  
- Endpoint → Actual URL path (e.g., `/customers/{id}`)

**Path Parameters:**  
- `/customers/{customerId}`  
- Dynamic, mandatory value to identify a specific resource  

---

## 7. Query Parameters

Example: `/tracking?trackingCode=SW123`  
- Optional  
- Used for filtering/searching  
- Not resource identity  

---

## 8. Nested Resources (Advanced REST)

Example: `/drivers/{driverId}/assignments`  
- Shows hierarchy and relationship  
- Assignment belongs to a driver  
- Common in enterprise systems  

---

## 9. Data Types

**Definition:** Schema defining:  
- Field names  
- Field types  
- Required vs optional  
- Validation rules  

**Example (Customer):**
```yaml
type: object
properties:
  name: string
  phone: string
  address: string
````

**Why it matters:** Ensures consistency and auto-validates requests.

---

## 10. Request Body vs Response Body

* **Request Body:** Data sent to the server (POST, PUT, PATCH)
* **Response Body:** Data sent from server (includes data, status message, metadata)

---

## 11. HTTP Status Codes

| Code | Meaning        |
| ---- | -------------- |
| 200  | Success        |
| 201  | Created        |
| 400  | Client mistake |
| 404  | Not found      |
| 500  | Server error   |

---

## 12. Traits (Reusability Concept)

* Reusable behaviors applied to multiple methods
* Examples: error responses, headers, security
* Follows **DRY principle**

---

## 13. Resource Types

* Define common patterns and methods
* Example: Collection resource `/customers`
* Ensures consistency and clean architecture

---

## 14. Examples

* Sample requests and responses
* Help frontend developers and testers
* APIs without examples are considered incomplete

---

## 15. Security Scheme (API Key)

* API Key = secret token identifying the caller
* Sent via `X-API-Key` header
* Simple, lightweight, common for internal APIs

---

## 16. Documentation Endpoint

* `/docs`
* Explains workflows, authentication, usage
* Reduces onboarding time

---

## 17. Overlay (Advanced RAML)

* Modifies documentation without changing base logic
* Supports localization (multi-language)

---

## 18. Annotations

* Custom metadata
* Non-functional information
* Used for governance, audit, lifecycle tracking

---

## 19. Extension File

* Adds endpoints without touching base RAML
* Supports safe, team-based development

---

## 20. MuleSoft Context

* RAML used to generate flows, validate payloads, auto-generate docs
* Enforces governance and standards

---

# Step-by-Step RAML Guide for SwiftCourier

## Folder Structure (VS Code)

```text
swift-courier-api/
│
├── swift-courier-api.raml        # Main RAML file
├── types/
│   ├── Customer.raml
│   ├── Parcel.raml
│   ├── Driver.raml
│   ├── Payment.raml
│   └── Tracking.raml
├── traits/
│   └── error-trait.raml
├── resourceTypes/
│   └── collection-get.raml
├── examples/
│   ├── customer-example.json
│   ├── parcel-example.json
│   ├── driver-example.json
│   ├── payment-example.json
│   └── tracking-example.json
├── annotations/
│   └── annotations.raml
├── overlay/
│   └── am-overlay.raml
└── swift-courier-api-ext.raml
```

---

## Main RAML File: `swift-courier-api.raml`

```yaml
#%RAML 1.0
title: SwiftCourier API
version: v1
description: REST API for parcel delivery service
protocols: [ HTTP, HTTPS ]
mediaType: application/json
```

* Describes **endpoints**, **data**, **responses**
* Does **NOT** connect to database or run code

---

## Example Resource: `/customers`

| Method | Meaning               |
| ------ | --------------------- |
| POST   | Create new customer   |
| GET    | Get list of customers |

**Single Customer:** `/customers/{customerId}`

* Dynamic ID to access specific customer

---

## Other Key Resources

* `/parcels` → Create, update, change status, delete parcel
* `/tracking?trackingCode=SC123` → Lookup by code
* `/drivers/{driverId}/assignments` → Nested assignments
* `/payments` → Transaction handling

---

## Types Folder (`types/`)

* Defines **data structures** for entities
* Example (Customer):

```yaml
type: object
properties:
  id: string
  name: string
  phone: string
  address: string
```

---

## Traits Folder (`traits/`)

* `error-trait.raml` → reusable error responses (400, 404, 500)

---

## Resource Types (`resourceTypes/`)

* Templates for resources like collection GET or item GET by ID

---

## Examples Folder (`examples/`)

* Sample JSON for testing and frontend guidance

---

## Security Scheme

* API Key authentication
* Header: `X-API-Key`

---

## Docs Resource: `/docs`

* Explains API usage for humans

---

## Overlay File (`overlay/am-overlay.raml`)

* Localizes documentation without changing base API

---

## Annotations File (`annotations/annotations.raml`)

* Adds governance, auditing, lifecycle info

---

## Extension File (`swift-courier-api-ext.raml`)

* Adds new endpoints like `/reports` without modifying base API

---

# Presentation Tips

1. Open VS Code → Open project folder
2. Show **folder structure** first
3. Explain **main RAML file**
4. Show **types**, **traits**, **resourceTypes** folders
5. Present **overlay** and **extension** last

### Most Important Viva Question

**Q:** Does this API run?
**A:**

> "This is a RAML API contract. It is designed first and later implemented using MuleSoft or backend services."

```


