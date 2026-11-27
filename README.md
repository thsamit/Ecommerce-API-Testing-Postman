# 🛒 E-Commerce API Testing with Postman

A complete API testing project built with **Postman**, automating end-to-end user authentication, product retrieval, and shopping cart workflows using dynamic environment variables and JavaScript test assertions.

---

## 📌 What is API Testing?

**API (Application Programming Interface) Testing** is a type of software testing that directly validates the backend logic, data responses, security, and performance of an application without needing a user interface (UI).

Instead of clicking buttons on a website, API testing sends requests directly to the server's endpoints and verifies:
* **Status Codes:** Did the server respond with the expected HTTP code (e.g., `200 OK`, `201 Created`, `401 Unauthorized`)?
* **Response Data (Payload):** Is the returned JSON data accurate, properly formatted, and complete?
* **Authorization & Security:** Are protected routes (like user profiles) blocked without a valid authentication token?
* **Chained Workflows:** Can data from one API response (like a login `accessToken` or `product_id`) be dynamically passed into subsequent API requests?

---

## 🌐 Common HTTP Methods (With Examples)

RESTful APIs rely on standard HTTP methods to perform **CRUD** (Create, Read, Update, Delete) operations:

| HTTP Method | Operation | Description | Example Endpoint & Usage |
| :--- | :--- | :--- | :--- |
| **`GET`** | **Read** | Retrieves data from a server without modifying anything. | `GET /products` — Fetches a list of all available store products. |
| **`POST`** | **Create** | Sends new data to the server to create a resource or trigger an action. | `POST /auth/login` — Sends `username` and `password` in a JSON body to generate an auth token. |
| **`PUT`** | **Update (Full)** | Replaces an entire existing resource with updated data. | `PUT /products/1` — Replaces all fields of product #1 with new details. |
| **`PATCH`** | **Update (Partial)** | Modifies only specific fields of an existing resource. | `PATCH /products/1` — Updates only the `price` of product #1. |
| **`DELETE`** | **Delete** | Removes a specific resource from the server. | `DELETE /carts/31` — Deletes cart #31 from the system. |

---

## 🚀 About This Project

This repository contains a Postman Collection and Environment designed to test a simulated E-Commerce backend (`https://dummyjson.com`). Rather than running isolated requests, this project chains API calls together using Postman's **Post-response Scripts**.

### ✨ Key Features
* **Automated Token Management:** Extracts the `accessToken` from the login response and automatically injects it as a Bearer Token into protected requests.
* **Dynamic Variable Chaining:** Captures `product_id` and `cart_id` from JSON responses and stores them in environment variables for downstream requests.
* **Automated Assertions:** Uses Postman's `pm.test()` and Chai assertions (`pm.expect`) to validate HTTP status codes (`200`, `201`) and verify response payload integrity.

### 📂 Repository Files
* `Ecommerce_API_Collection.postman_collection.json` — Contains all 6 API requests, request bodies, authorization headers, and JavaScript test scripts.
* `Ecommerce_API_Env.postman_environment.json` — Stores environment variables (`base_url`, `token`, `product_id`, `cart_id`).

### 🔗 Tested Endpoints & Workflow

| # | Request Name | Method | Endpoint | Automated Tests & Actions |
| :- | :--- | :--- | :--- | :--- |
| 1 | **User Login** | `POST` | `{{base_url}}/auth/login` | Verifies status `200`; extracts `accessToken` and saves it to `{{token}}`. |
| 2 | **Get User Profile** | `GET` | `{{base_url}}/auth/me` | Uses `{{token}}` (Bearer Auth); verifies status `200`. |
| 3 | **Get Products** | `GET` | `{{base_url}}/products` | Verifies status `200`; extracts the first product's ID into `{{product_id}}`. |
| 4 | **Get Single Product** | `GET` | `{{base_url}}/products/{{product_id}}` | Dynamically fetches details for the saved `{{product_id}}`. |
| 5 | **Add to Cart** | `POST` | `{{base_url}}/carts/add` | Adds `{{product_id}}` to a cart; verifies status `201`, checks product array length $> 0$, and saves `{{cart_id}}`. |
| 6 | **Get Cart Details** | `GET` | `{{base_url}}/carts/31` | Verifies status `200` and asserts that the cart `total` is greater than `0`. |

---

## 🛠️ How to Clone and Run This Project

Follow these steps to run the test suite on your own machine:

1. **Clone or Download the Repository:**
   ```bash
   git clone https://github.com/thsamit/Ecommerce-API-Testing-Postman.git
   ```
   *(Or click **Code** > **Download ZIP** on GitHub and extract the folder.)*

2. **Import into Postman:**
   * Open the **Postman** desktop app.
   * Click **Import** in the top-left sidebar.
   * Drag and drop both `Ecommerce_API_Collection.postman_collection.json` and `Ecommerce_API_Env.postman_environment.json` into the import window.

3. **Select the Active Environment:**
   * In the top-right corner of Postman, click the environment dropdown (which says *No Environment* by default) and select **`Ecommerce_API_Env`**.
   * Ensure `base_url` is set to `https://dummyjson.com`.

4. **Run the Collection:**
   * Click the three dots (`...`) next to **`Ecommerce_API_Collection`** and select **Run** (or run each request sequentially starting from **User Login**).
   * View the **Test Results** tab to verify that all automated assertions pass!

---

## 🧰 Tools & Technologies Used
* **Postman** — API client, environment management, and Collection Runner
* **JavaScript (Postman Sandbox API)** — Automated test assertions (`pm.test`, `pm.expect`) and variable scripting (`pm.environment.set`)
* **JSON** — Request payloads and response validation
