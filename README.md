```markdown
# Token-Based Authentication Backend (Flask + Postman)

## 1. Experiment Title

Implementation and Testing of Token-Based Authentication Mechanisms using Flask and Postman.

---

## 2. Objective

- To develop a backend server in Python using the Flask framework.  
- To implement three different token-based authentication approaches:
  1. Username/Password via Authorization header  
  2. Username/Password via a custom header  
  3. Username/Password with JWT, used as a Bearer token  
- To test and validate all authentication flows using Postman.

---

## 3. Problem Statement

Modern web applications require secure and stateless mechanisms to authenticate users.  
In this experiment, a simple Flask-based backend is created to compare multiple authentication styles—header-based credentials and JWT-based tokens—and to observe how clients (like Postman) interact with these mechanisms via HTTP requests and headers.

---

## 4. System Requirements

- Python 3.x  
- Flask  
- PyJWT (or any JWT library)  
- Postman (or any REST client)  
- Basic understanding of HTTP methods (GET, POST), headers, and JSON

---

## 5. Implementation Details

### 5.1 Project Structure

```text
auth-backend-flask/
│
├─ app.py              # Main Flask application with routes
├─ requirements.txt    # Dependencies (Flask, PyJWT, etc.)
└─ README.md           # Documentation
```

`requirements.txt` (example):

```text
Flask
PyJWT
```

### 5.2 Common Setup

- Create an in-memory user (no database for simplicity), e.g.:
  - `username = "admin"`
  - `password = "password123"`
- Initialize Flask application in `app.py`.
- Add a health-check endpoint:
  - `GET /health` → returns simple JSON `{ "status": "ok" }`.

---

## 6. Authentication Endpoints

### 6.1 Authentication via Authorization Header

**Endpoint:** `POST /auth/basic`  

**Description:**  
Reads username and password from the `Authorization` header, validates them against the in-memory user, and returns success or failure.

**Key points:**

- Expected header format can be:
  - `Authorization: Basic <encoded-credentials>` or
  - `Authorization: <custom-format>`
- Server:
  - Parses the header, extracts credentials.
  - Compares with stored username/password.
  - Returns:
    - `200 OK` with success message on valid credentials.
    - `401 Unauthorized` with error message on invalid/missing credentials.

**Postman usage:**

- Method: `POST`
- URL: `http://localhost:5000/auth/basic`
- Headers: set appropriate `Authorization` value.
- Check both valid and invalid cases.

---

### 6.2 Authentication via Custom Header

**Endpoint:** `POST /auth/custom`  

**Description:**  
Uses custom headers for sending credentials instead of using the Authorization header.

**Headers example:**

- `X-Username: admin`  
- `X-Password: password123`

**Server behavior:**

- Reads `X-Username` and `X-Password` from request headers.
- Validates them against the in-memory user.
- Returns:
  - `200 OK` and success message when credentials match.
  - `401 Unauthorized` otherwise.

**Postman usage:**

- Method: `POST`
- URL: `http://localhost:5000/auth/custom`
- Add custom headers with valid and then invalid values to observe responses.

---

### 6.3 JWT-Based Authentication (Bearer Token)

This approach consists of a login endpoint and a protected endpoint.

#### 6.3.1 Login Endpoint (JWT Issuance)

**Endpoint:** `POST /auth/jwt/login`  

**Description:**  
Accepts username and password in the request body, validates them, and returns a signed JWT token.

**Request body (JSON):**

```json
{
  "username": "admin",
  "password": "password123"
}
```

**Server behavior:**

- Validates credentials against in-memory user.
- If valid:
  - Generates JWT with:
    - Subject/identifier (e.g., username or user_id).
    - Expiration time (`exp` claim).
  - Returns JSON:
    ```json
    {
      "access_token": "<jwt-token>"
    }
    ```
- If invalid:
  - Returns `401 Unauthorized` with error message.

**Postman usage:**

- Method: `POST`
- URL: `http://localhost:5000/auth/jwt/login`
- Body → `raw` → `JSON` with username and password.
- Save the `access_token` for next step.

#### 6.3.2 Protected Endpoint (Bearer Token Validation)

**Endpoint:** `GET /protected`  

**Description:**  
A protected resource that can only be accessed with a valid JWT provided in the `Authorization` header as a Bearer token.

**Header format:**

- `Authorization: Bearer <jwt-token>`

**Server behavior:**

- Extracts token from the `Authorization` header.
- Verifies signature and checks expiration.
- If token is valid:
  - Returns `200 OK` and a message with user info.
- If token is missing, invalid, or expired:
  - Returns `401 Unauthorized` with appropriate error.

**Postman usage:**

- Method: `GET`
- URL: `http://localhost:5000/protected`
- Set `Authorization` header with Bearer token:
  - `Bearer <access_token>`
- Test three scenarios:
  - Valid token.
  - Modified/invalid token.
  - No token.

---

## 7. Testing Procedure (Postman)

1. Start Flask server (`python app.py`).  
2. Test `GET /health` to ensure backend is running.  
3. Test `POST /auth/basic`:
   - With correct Authorization header → expect `200`.  
   - With wrong/missing header → expect `401`.  
4. Test `POST /auth/custom`:
   - Correct `X-Username` and `X-Password` → `200`.  
   - Wrong/missing headers → `401`.  
5. Test `POST /auth/jwt/login`:
   - Correct credentials → receives `access_token`.  
   - Wrong credentials → `401`.  
6. Test `GET /protected`:
   - With valid Bearer token → `200` and protected message.  
   - With invalid or missing token → `401`.

---

## 8. Learning Outcomes

By completing this experiment, the student will be able to:

1. **Flask Backend Basics**
   - Set up a basic Flask server and define routes for different HTTP methods.
   - Return JSON responses and manage HTTP status codes.

2. **Understanding HTTP Headers**
   - Explain the role of headers in HTTP requests (Authorization, custom headers).
   - Implement authentication logic using data passed through headers instead of the body.

3. **Token-Based Authentication Concepts**
   - Differentiate between simple header-based credential checks and token-based mechanisms.
   - Understand why tokens are preferred over sending raw credentials for every request.

4. **JSON Web Tokens (JWT)**
   - Generate JWTs on successful login and configure claims such as subject and expiration.
   - Verify and decode JWTs on subsequent requests to authorize access to protected resources.
   - Recognize how JWT enables stateless authentication on the server side.

5. **Security Awareness**
   - Understand risks of sending plain-text credentials and the need for secure transport (HTTPS).
   - Appreciate the importance of secret keys, expiration times, and proper error handling in auth flows.

6. **API Testing with Postman**
   - Use Postman to send requests with custom headers and body payloads.
   - Interpret responses, headers, and status codes to debug authentication issues.
   - Systematically test success and failure paths for all authentication methods.

7. **Design Comparison and Best Practices**
   - Compare Authorization header, custom header, and JWT Bearer approaches in terms of usability and security.
   - Identify which mechanisms are more suitable for production APIs and why (e.g., JWT Bearer tokens instead of plain credentials).

---

## 9. Conclusion

This experiment demonstrates how different token-based authentication methods can be implemented in a Flask backend and validated via Postman.  
By working through all three approaches, students gain practical experience with HTTP headers, JWTs, and secure access control patterns used in real-world web services.
```