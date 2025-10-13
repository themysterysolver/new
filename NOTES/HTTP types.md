Here’s a **complete guide to the types of HTTP requests** — also called **HTTP methods** — commonly used in web development:

---

## **1️⃣ GET**

* **Purpose:** Retrieve data from the server.
* **Body:** No request body (parameters sent in URL query string).
* **Idempotent:** Yes (multiple requests have same effect).
* **Example:**

```http
GET /users/123 HTTP/1.1
Host: example.com
```

* **Use case:** Fetch a webpage, API data, or user info.

---

## **2️⃣ POST**

* **Purpose:** Send data to the server to **create a resource**.
* **Body:** Contains data (JSON, form data, etc.).
* **Idempotent:** No (multiple requests create multiple resources).
* **Example:**

```http
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

* **Use case:** Submitting forms, creating a new user, uploading files.

---

## **3️⃣ PUT**

* **Purpose:** **Replace or update** a resource entirely.
* **Body:** Contains new state of the resource.
* **Idempotent:** Yes (same request multiple times has same effect).
* **Example:**

```http
PUT /users/123 HTTP/1.1
Content-Type: application/json

{
  "name": "Alice Smith",
  "email": "alice.smith@example.com"
}
```

* **Use case:** Updating all fields of a user or object.

---

## **4️⃣ PATCH**

* **Purpose:** **Partially update** a resource.
* **Body:** Contains only the changes.
* **Idempotent:** Yes (same request multiple times has same effect).
* **Example:**

```http
PATCH /users/123 HTTP/1.1
Content-Type: application/json

{
  "email": "alice.new@example.com"
}
```

* **Use case:** Updating only one field (like email) without changing the rest.

---

## **5️⃣ DELETE**

* **Purpose:** Remove a resource.
* **Body:** Usually empty.
* **Idempotent:** Yes (deleting the same resource multiple times has same effect).
* **Example:**

```http
DELETE /users/123 HTTP/1.1
```

* **Use case:** Delete a user, post, or record.

---

## **6️⃣ HEAD**

* **Purpose:** Retrieve headers of a resource **without the body**.
* **Body:** None.
* **Idempotent:** Yes.
* **Example:**

```http
HEAD /users/123 HTTP/1.1
```

* **Use case:** Check if resource exists, content type, or last-modified date.

---

## **7️⃣ OPTIONS**

* **Purpose:** Ask the server which HTTP methods are **allowed** for a resource.
* **Body:** Optional.
* **Idempotent:** Yes.
* **Example:**

```http
OPTIONS /users HTTP/1.1
```

* **Use case:** CORS preflight requests, API exploration.

---

## **8️⃣ TRACE**

* **Purpose:** Echo back the received request for diagnostic purposes.
* **Idempotent:** Yes.
* **Example:**

```http
TRACE /users HTTP/1.1
```

* **Use case:** Debugging or testing how the request is received by the server.

---

## **9️⃣ CONNECT**

* **Purpose:** Establish a tunnel to the server (commonly used for HTTPS through a proxy).
* **Idempotent:** Yes.
* **Example:**

```http
CONNECT proxy.example.com:443 HTTP/1.1
```

* **Use case:** Proxy tunneling for SSL connections.

---

## ✅ **Summary Table**

| Method  | Purpose                   | Body       | Idempotent | Use Case                     |
| ------- | ------------------------- | ---------- | ---------- | ---------------------------- |
| GET     | Retrieve data             | No         | Yes        | Fetch pages or API data      |
| POST    | Create resource           | Yes        | No         | Submit forms, create objects |
| PUT     | Update/replace resource   | Yes        | Yes        | Replace entire object        |
| PATCH   | Partially update resource | Yes        | Yes        | Update single fields         |
| DELETE  | Delete resource           | Usually no | Yes        | Remove a record              |
| HEAD    | Retrieve headers          | No         | Yes        | Check existence or metadata  |
| OPTIONS | Check allowed methods     | Optional   | Yes        | CORS, API info               |
| TRACE   | Echo request              | No         | Yes        | Debugging                    |
| CONNECT | Open tunnel               | No         | Yes        | Proxy/HTTPS tunneling        |

Here’s a **practical guide to using HTTP methods with Express.js**, with examples for each method. Express is a popular Node.js framework for building APIs and web apps.

---

## **1️⃣ Setup Express App**

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON
app.use(express.json());

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

---

## **2️⃣ GET Request**

**Purpose:** Retrieve data

```javascript
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  // Example: fetch user from database
  res.json({ id: userId, name: 'Alice' });
});
```

**Test:**

```
GET http://localhost:3000/users/1
```

---

## **3️⃣ POST Request**

**Purpose:** Create a new resource

```javascript
app.post('/users', (req, res) => {
  const newUser = req.body; // {name: "Bob", email: "bob@example.com"}
  // Example: save to database
  res.status(201).json({ message: 'User created', user: newUser });
});
```

**Test:**

```
POST http://localhost:3000/users
Body: { "name": "Bob", "email": "bob@example.com" }
```

---

## **4️⃣ PUT Request**

**Purpose:** Replace/update an existing resource

```javascript
app.put('/users/:id', (req, res) => {
  const userId = req.params.id;
  const updatedUser = req.body;
  // Example: replace user in database
  res.json({ message: `User ${userId} updated`, user: updatedUser });
});
```

**Test:**

```
PUT http://localhost:3000/users/1
Body: { "name": "Alice Smith", "email": "alice.smith@example.com" }
```

---

## **5️⃣ PATCH Request**

**Purpose:** Partially update a resource

```javascript
app.patch('/users/:id', (req, res) => {
  const userId = req.params.id;
  const changes = req.body;
  // Example: update specific fields in database
  res.json({ message: `User ${userId} patched`, changes });
});
```

**Test:**

```
PATCH http://localhost:3000/users/1
Body: { "email": "alice.new@example.com" }
```

---

## **6️⃣ DELETE Request**

**Purpose:** Delete a resource

```javascript
app.delete('/users/:id', (req, res) => {
  const userId = req.params.id;
  // Example: remove user from database
  res.json({ message: `User ${userId} deleted` });
});
```

**Test:**

```
DELETE http://localhost:3000/users/1
```

---

## **7️⃣ HEAD Request**

**Purpose:** Get headers only

```javascript
app.head('/users/:id', (req, res) => {
  res.set('Custom-Header', 'HeaderValue');
  res.status(200).end(); // Send headers without body
});
```

---

## **8️⃣ OPTIONS Request**

**Purpose:** Discover allowed methods

```javascript
app.options('/users', (req, res) => {
  res.set('Allow', 'GET,POST,PUT,PATCH,DELETE,OPTIONS');
  res.send();
});
```

---

## ✅ **Testing Tips**

* Use **Postman** or **curl** to test each HTTP method.
* Example curl for POST:

```bash
curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"name":"Bob"}'
```

---



If you want, I can make a **visual diagram showing all HTTP methods with examples** — it’s super helpful for memorizing them. Do you want me to do that?
