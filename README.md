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
