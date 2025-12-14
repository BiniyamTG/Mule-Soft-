#  MuleSoft RAML API Design – SwiftCourier [RAML]

## 1. API (Application Programming Interface)

**What an API really is (deep meaning)**  
An API is a **contract** that defines how two software systems communicate.

Think of it like a **restaurant analogy**:  
- **Menu** → API  
- **Kitchen** → Backend system  
- **Customer** → Frontend or another system  

**Menu / API explains:**  
- What you can request  
- How to request it  
- What you will receive  

> **Important:** The API does not explain *how the food is cooked*—just what is available and how to order it.  

That is exactly what your **RAML file does**.

---

## 2. REST (Representational State Transfer)

**What REST really means**  
REST is an **architectural style**, not a programming language.  
It says: *“Design APIs using real-world resources and standard rules.”*

### 2.1 Core REST Ideas

#### Resources
A resource is a **thing**, not an action.

**Examples:**  
- Customer  
- Parcel  
- Driver  
- Payment  

**Bad design:** `/createCustomer`  
**Good REST design:** `/customers`

#### HTTP Methods (Verbs)
REST separates:  
- **WHAT** you act on → Resource (noun)  
- **HOW** you act → HTTP method (verb)  

| Method | Meaning               |
|--------|----------------------|
| GET    | Read data (no change)|
| POST   | Create new data      |
| PUT    | Replace entire resource |
| PATCH  | Update part of resource |
| DELETE | Remove resource      |

**Benefit:** APIs become predictable, scalable, and easy to learn.

#### Statelessness
**Stateless** = Every request contains all required information; the server does **not** remember previous requests.  

**Why it matters:**  
- Better performance  
- Easier scaling  
- Safer systems  

---

## 3. RAML (RESTful API Modeling Language)

**What RAML really is**  
RAML is:  
- A **design language**  
- A **specification / blueprint**  

> **It is NOT executable code.**  

RAML answers:  
- What endpoints exist?  
- What data is accepted?  
- What data is returned?  
- What errors are possible?  

### Why RAML uses YAML
YAML is:  
- Human-readable  
- Indentation-based  
- Clean and minimal  

**Benefit:** Developers, architects, and even non-programmers can understand API design.

---

## 4. API-First Design (VERY IMPORTANT)

### Traditional Approach (BAD)
1. Write backend code  
2. Expose endpoints later  
3. Frontend waits  
4. Misunderstandings & bugs  

### API-First Approach (GOOD)
1. Design API contract (RAML) first  
2. Team agrees on it  
3. Backend & frontend work in parallel  
4. Fewer bugs and conflicts  

> **Why companies love RAML:** it enables API-first development.

---

## 5. API Metadata

**Metadata = data about data**  
It describes:  
- Identity of the API  
- How it communicates  
- Version rules  

### Versioning (v1)
- APIs evolve over time  
- Old clients must not break  
- New features added safely  

**Example:**  
```text
/v1/customers
/v2/customers
````

> Without versioning → production disaster.

### Media Type (application/json)

* Tells **what format** the data is in
* JSON is lightweight, language-independent, human-readable, and industry standard

---

## 6. Resources and Endpoints

**Resource vs Endpoint**

* Resource → Conceptual entity (e.g., Customer)
* Endpoint → Actual URL path (e.g., `/customers/{id}`)

### Path Parameters

* `{customerId}` is dynamic and identifies a specific resource
* Mandatory when you want **one exact thing**

---

## 7. Query Parameters

**Example:** `/tracking?trackingCode=SW123`

* Optional
* Used for filtering/searching
* Not a resource identity

> Perfect for tracking user-friendly searches.

---

## 8. Nested Resources (Advanced REST)

**Example:** `/drivers/{driverId}/assignments`

* Assignments belong to a driver
* Shows relationship (resource hierarchy)
* Used in enterprise systems, microservices, real-world modeling

---

## 9. Data Types

**What a Data Type really is**

* Defines fields, types, required vs optional, validation rules

**Example:**

```yaml
Customer:
  name: string
  phone: string
  address: string
```

**Benefit:** Prevents invalid data, ensures consistency, auto-validates requests.

---

## 10. Request Body vs Response Body

* **Request Body:** Data sent to the server (POST, PUT, PATCH)
* **Response Body:** Data returned by the server, includes actual data, status message, metadata

---

## 11. HTTP Status Codes

| Code | Meaning        |
| ---- | -------------- |
| 200  | Success        |
| 201  | Created        |
| 400  | Client mistake |
| 404  | Not found      |
| 500  | Server error   |

> Universal rules, not MuleSoft-specific.

---

## 12. Traits (Reusability Concept)

**Trait = reusable behavior applied to multiple methods**

* Example: error responses, headers, security rules
* Follow **DRY principle** (Don’t Repeat Yourself)

---

## 13. Resource Types

**Resource Types = templates**

* Collection resource: `/customers`
* Item resource: `/customers/{id}`

> Ensures consistency and clean architecture.

---

## 14. Examples (Why They Matter)

* Sample requests & responses
* Shows practical understanding, helps testing, improves documentation

> APIs without examples are incomplete.

---

## 15. Security Scheme (API Key)

* API key = secret token identifying the caller
* Sent via `X-API-Key`
* Simple, lightweight, common for internal APIs

---

## 16. Documentation Endpoint

**Example:** `/docs`

* Explains workflows
* Reduces onboarding time
* Good APIs always include documentation

---

## 17. Overlay (Advanced RAML)

* Modifies documentation without changing logic
* Supports localization (language/region-specific info)

---

## 18. Annotations

* Custom metadata, non-functional
* Used for governance, audit, API lifecycle tracking
* Examples: `@createdBy`, `@deprecated`

---

## 19. Extension File

* Add new endpoints without touching base RAML
* Keeps base contract stable
* Teams extend independently

---

## 20. MuleSoft Context

RAML in MuleSoft helps:

* Generate flows
* Validate payloads
* Auto-generate docs
* Enforce governance

> This is exactly how real MuleSoft projects start.

---

# 📝 Add-Up Note



### What Problem Does RAML Solve?

* Without RAML: chaos, miscommunication, bugs, delays
* With RAML: **ONE SOURCE OF TRUTH** for endpoints, data, errors

### What is RAML? (Simple)

**RAML = RESTful API Modeling Language**

* RESTful → follows REST rules
* API → communication contract
* Modeling → blueprint/design
* Language → written format

> RAML is **DESIGN ONLY**, not executable code

**Analogy:**

* Blueprint ≠ house
* RAML ≠ backend

### Why Companies Use RAML

* Supports API-first development
* Improves collaboration
* Auto documentation
* Works with MuleSoft
* Prevents integration errors

### RAML + YAML

* Human-readable, indentation matters
* Example:

```yaml
title: SwiftCourier API
version: v1
```

### RAML File Header

```yaml
#%RAML 1.0
```

> Tells tools the file is RAML 1.0. Without it → invalid.

---

## Core RAML Concepts Summary

| Concept               | Meaning / Use                      |
| --------------------- | ---------------------------------- |
| Resources             | Things, not actions                |
| Endpoints             | Actual URL paths                   |
| HTTP Methods          | GET/POST/PUT/PATCH/DELETE          |
| Request Body          | Input data                         |
| Response Body         | Output data                        |
| Status Codes          | Communication signals              |
| Data Types            | Schema / validation                |
| Traits                | Reusable logic                     |
| Resource Types        | Templates for consistency          |
| Query Parameters      | Filtering / search                 |
| Examples              | Sample requests/responses          |
| Security Schemes      | API protection                     |
| Documentation `/docs` | Human-readable guide               |
| Overlays              | Modify docs without changing logic |
| Extensions            | Add endpoints safely               |
| Annotations           | Governance & metadata              |

---
