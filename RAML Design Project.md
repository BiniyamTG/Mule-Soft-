# MuleSoft Assignment: Real-World RAML Design Project

## Project Title
**Design a RESTful API for an Online Parcel Delivery & Courier Service**

## Scenario
You have been hired as an **API Designer** for **SwiftCourier**, a parcel delivery and logistics company. Your responsibility is to **design the API contract using RAML**.

---

## Assignment Requirements

### 1. Create a RAML API Specification
**File name:** `swift-courier-api.raml`

#### A. API Metadata
Include the following metadata:
- **Title**  
- **Version:** v1  
- **Description**  
- **Protocols:** HTTP / HTTPS  
- **Media Types:** `application/json`  

#### B. Resources & Methods

1. **/customers**
   - `POST` – Create customer  
   - `GET` – List customers  
   - `/customers/{customerId}`  
     - `GET` – Get customer details  
     - `PUT` – Update customer  
     - `DELETE` – Remove customer  

2. **/parcels**
   - `POST` – Create parcel order  
   - `GET` – Get all parcels  
   - `/parcels/{parcelId}`  
     - `GET` – Get parcel details  
     - `PUT` – Update parcel  
     - `PATCH` – Partial update  
     - `DELETE` – Remove parcel  

3. **/tracking**
   - `GET` – Track parcel using `trackingCode` query parameter  

4. **/drivers**
   - `GET` – List drivers  
   - `POST` – Register driver  
   - `/drivers/{driverId}/assignments`  
     - `GET` – Driver's assigned deliveries  
     - `POST` – Assign parcel to driver  

5. **/payments**
   - `POST` – Make payment  
   - Attributes: `method`, `amount`, `parcelId`, `status`  

---

#### C. Traits & Resource Types
- Use a **trait** for error responses (`400`, `404`, `500`)  
- Use at least **one resourceType** (GET collection, GET by ID)  

---

#### D. Data Types
Create data types for:  
- Customer  
- Parcel  
- Driver  
- Payment  
- Tracking  

---

#### E. Examples
Include **request and response examples** for all methods.  

---

#### F. Security
Implement **one security scheme**:  
- OAuth2, Basic Auth, or API Key header  

---

#### G. Documentation Section
Add a `/docs` resource explaining:  
- API purpose  
- Authentication  
- Tracking workflow  

---

#### H. Additional Requirements
- Add **RAML Overlays** for a second language (Amharic)  
- Create **Annotation Types**  
- Create an **Extension file**  

---

### Overlays
- Create a folder `/overlay` with a file such as `am-overlay.raml`  
- This file should:  
  - Translate documentation strings  
  - Override API descriptions  
  - Override `displayName` fields  
  - Add localized examples  

---

### Annotation Types
- Create `/annotations/annotations.raml` defining annotation types:  
  - `@createdBy`  
  - `@lastUpdated`  
  - `@deprecatedReason`  
- Apply these annotations throughout the API  

---

### Extension
- Create `swift-courier-api-ext.raml` which:  
  - Adds additional endpoints like `/reports`  

---

## Deliverables
1. RAML file  
2. Examples folder  
3. Types folder  
4. Optional: Postman collection  

---

## Learning Outcomes
- API design  
- RAML structure  
- API-first approach  
- Reusable components  
- Preparing for MuleSoft implementation




Absolutely! Let’s go **step by step** and create a **detailed, complete guide** for your **SwiftCourier RAML project**, including **project structure, file examples, and explanations** so you understand **everything**. I’ll break it down in **layman-friendly but technical detail**.

---

# 🛠️ Complete Guide: SwiftCourier RAML Project

## 1. Project Overview

**Project Goal:**
Design a **RESTful API** for an online parcel delivery and courier service called **SwiftCourier** using **RAML**. This project focuses on **API-first design**, defining the contract before implementation.

**Why this is important:**

* Standardizes API design
* Makes frontend & backend development parallel
* Reduces errors and misunderstandings
* Enables automated documentation & MuleSoft integration

**Scenario Recap:**
You are the **API Designer**. You will define endpoints, data types, security, traits, overlays, annotations, and examples in RAML.

---

## 2. Project Structure

Here is a **typical project folder structure**:

```
swiftcourier-raml/
│
├── swift-courier-api.raml        # Main RAML API specification
├── swift-courier-api-ext.raml    # Extension RAML (new endpoints)
│
├── types/                        # Data types definitions
│   ├── customer.raml
│   ├── parcel.raml
│   ├── driver.raml
│   ├── payment.raml
│   └── tracking.raml
│
├── examples/                      # Sample requests & responses
│   ├── customers/
│   │   ├── create-customer.json
│   │   ├── get-customer.json
│   │   └── list-customers.json
│   ├── parcels/
│   └── drivers/
│
├── annotations/                   # Custom annotation types
│   └── annotations.raml
│
└── overlay/                        # Overlays (second language)
    └── am-overlay.raml
```

> **Explanation:**

* `swift-courier-api.raml` → main API blueprint
* `types/` → all data structures (schemas)
* `examples/` → helps testers and frontend developers
* `annotations/` → custom metadata for governance or auditing
* `overlay/` → for translation/localization (Amharic)
* `swift-courier-api-ext.raml` → adds new endpoints without modifying base RAML

---

## 3. RAML File: `swift-courier-api.raml`

**Header & Metadata:**

```yaml
#%RAML 1.0
title: SwiftCourier API
version: v1
description: REST API for online parcel delivery & courier service
protocols: [HTTP, HTTPS]
mediaType: application/json
```

> **Explanation:**

* `#%RAML 1.0` → declares RAML version
* `title` → API name
* `version` → API version
* `description` → purpose
* `protocols` → supported transport protocols
* `mediaType` → default response/request format

---

## 4. Defining Resources & Methods

### 4.1 Customers Resource

```yaml
/customers:
  get:
    description: Retrieve list of all customers
    responses:
      200:
        body:
          application/json:
            example: !include examples/customers/list-customers.json
  post:
    description: Create a new customer
    body:
      application/json:
        type: Customer
        example: !include examples/customers/create-customer.json
    responses:
      201:
        body:
          application/json:
            example: !include examples/customers/get-customer.json

/customers/{customerId}:
  get:
    description: Retrieve customer details
    responses:
      200:
        body:
          application/json:
            example: !include examples/customers/get-customer.json
  put:
    description: Update customer information
  delete:
    description: Delete a customer
```

> **Explanation:**

* `get`, `post`, `put`, `delete` → HTTP verbs (actions)
* `{customerId}` → **path parameter** for dynamic resource access
* `example` → shows sample JSON request or response

---

### 4.2 Parcels Resource

```yaml
/parcels:
  get:
    description: Get all parcels
  post:
    description: Create a parcel order

/parcels/{parcelId}:
  get: Retrieve specific parcel
  put: Update parcel
  patch: Partial update
  delete: Remove parcel
```

---

### 4.3 Tracking Resource

```yaml
/tracking:
  get:
    description: Track a parcel by trackingCode
    queryParameters:
      trackingCode:
        type: string
        required: true
    responses:
      200:
        body:
          application/json:
            example:
              trackingCode: "SW123"
              status: "In Transit"
```

> **Query parameter:** optional or required for filtering/searching

---

### 4.4 Drivers Resource

```yaml
/drivers:
  get: List all drivers
  post: Register a driver

/drivers/{driverId}/assignments:
  get: Get all assigned parcels for a driver
  post: Assign a parcel to driver
```

---

### 4.5 Payments Resource

```yaml
/payments:
  post:
    description: Make a payment
    body:
      application/json:
        type: Payment
        example:
          method: "Credit Card"
          amount: 100
          parcelId: "P001"
          status: "Completed"
```

---

## 5. Data Types (Schemas)

Create these in `/types` folder.

**Example: Customer**

```yaml
# types/customer.raml
type: object
properties:
  id: string
  name: string
  phone: string
  address: string
  email: string
required: [id, name, phone]
```

**Other types:**

* `Parcel` → id, senderId, receiverId, weight, status
* `Driver` → id, name, phone, licenseNumber
* `Payment` → method, amount, parcelId, status
* `Tracking` → trackingCode, parcelId, status, location

---

## 6. Traits (Reusable Behavior)

```yaml
# Define error responses
traits:
  errorResponses:
    responses:
      400:
        description: Bad Request
      404:
        description: Not Found
      500:
        description: Internal Server Error
```

> **Usage:** Apply to multiple resources to avoid repeating responses.

---

## 7. Resource Types (Templates)

```yaml
resourceTypes:
  collection:
    get:
      description: Retrieve all <<resourceName>>
  item:
    get:
      description: Retrieve a single <<resourceName>> by ID
```

> **Benefit:** Maintain consistency across resources.

---

## 8. Examples

Place JSON files in `/examples` folder for each endpoint.

* `examples/customers/create-customer.json`

```json
{
  "id": "C001",
  "name": "John Doe",
  "phone": "0912345678",
  "address": "Addis Ababa"
}
```

---

## 9. Security Scheme

```yaml
securitySchemes:
  api_key:
    description: API Key header for authentication
    type: x-api-key
    describedBy:
      headers:
        X-API-Key:
          description: API key value
          type: string
```

---

## 10. Documentation Resource

```yaml
/docs:
  get:
    description: |
      API documentation including:
      - Purpose of API
      - Authentication
      - Tracking workflow
```

---

## 11. Overlays (Amharic)

* File: `overlay/am-overlay.raml`
* Override descriptions, displayNames, and examples in Amharic

```yaml
# overlay/am-overlay.raml
title: "SwiftCourier ኤፒአይ"
description: "ኤፒአይ ለመላኪያና ኮርየር አገልግሎት"
```

---

## 12. Annotations

* File: `/annotations/annotations.raml`

```yaml
annotations:
  createdBy:
    type: string
  lastUpdated:
    type: string
  deprecatedReason:
    type: string
```

* Apply to resources or methods:

```yaml
/customers:
  (createdBy): "Biniyam Teketel"
  get: ...
```

---

## 13. Extension File

* File: `swift-courier-api-ext.raml`
* Add new endpoints like `/reports` without touching base RAML

```yaml
/reports:
  get:
    description: Retrieve API reports
```

---

## 14. Deliverables

1. `swift-courier-api.raml` (main RAML file)
2. `/examples` folder with JSON request/response examples
3. `/types` folder with data types
4. Optional: Postman collection for testing

---

## 15. Learning Outcomes

* Understand **API-first design**
* Master **RAML structure**
* Define **resources, traits, resourceTypes, data types**
* Apply **security schemes**
* Create **overlays, annotations, and extension files**
* Prepare for **MuleSoft implementation**

---

