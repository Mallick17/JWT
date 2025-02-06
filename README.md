## JSON Web Tokens
- JWT is an open standard (RFC 7519) that defines a compact, self-contained way to securely transmit information between parties as a JSON object.
- **Purpose:**
  - It is commonly used for authentication and authorization in web applications.
- **Key Features**
  - **Compact:**  JWTs are small in size, making them easy to transmit via URLs, POST requests, or HTTP headers.
  - **Self-Contained:** The token contains all the necessary information about the user, reducing the need to query the database repeatedly.
  - **Digitally Signed:**  JWTs can be signed using a secret (HMAC) or a public/private key pair (RSA or ECDSA), ensuring the integrity of the data.

## Why use JWT?
- **Authentication**
  - JWTs are commonly used to authenticate users. Once a user logs in, a JWT is issued and sent to the client. The client includes this token in subsequent requests to access protected resources.
- **Authorization**
  - JWTs can contain user roles and permissions, allowing servers to authorize access to specific resources.
- **Advantages**
  - **Stateless:** JWTs are self-contained, so the server doesn’t need to store session data.
  - **Cross-Domain:** JWTs can be used across different domains, making them ideal for distributed systems.
  - **Reduced Overhead:** Eliminates the need to send login credentials (e.g., username and password) with every request.

## Structure of a JWT
- A JWT consists of three parts, separated by dots (.):
1. **Header**
   - Contains metadata about the token, such as the signing algorithm (e.g., HMAC SHA256) and the type of token (JWT).
     ```json
     {
       "alg": "HS256",
       "typ": "JWT"
     }
     ```
2. **Payload**
   - Contains the claims, which are statements about the user (e.g., user ID, email, roles) and additional data (e.g., expiration time).
     ```json
     {
       "sub": "1234567890",
       "name": "John Doe",
       "admin": true
     }
     ```
3. **Signature**
   - Ensures the integrity of the token. It is created by signing the encoded header and payload using a secret key.
     ```json
     HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
     ```
## JWT Workflow
1. **User Login**:
   - The client sends login credentials (e.g., email and password) to the server.
   - The server verifies the credentials and generates a JWT (access token) and optionally a refresh token.
   - The access token is sent to the client, and the refresh token is stored securely (e.g., in an HTTP-only cookie).

2. **Accessing Protected Resources**:
   - The client includes the JWT in the `Authorization` header of subsequent requests.
   - The server verifies the JWT’s signature and checks its claims (e.g., expiration time) to authorize the request.

3. **Token Expiry and Refresh**:
   - Access tokens have a short lifespan (e.g., 15 minutes) for security reasons.
   - Refresh tokens have a longer lifespan (e.g., 7 days) and are used to obtain a new access token when the current one expires.

## **Implementation Details**
- **Backend (Server)**:
  - Use libraries like `jsonwebtoken` to create and verify JWTs.
  - Store refresh tokens securely (e.g., in a database or HTTP-only cookies).
  - Protect routes by verifying the JWT in the `Authorization` header.

- **Frontend (Client)**:
  - Store the access token in memory or local storage (though local storage is less secure).
  - Include the access token in the `Authorization` header for protected API requests.
  - Use refresh tokens to obtain new access tokens without requiring the user to log in again.
    
## **Security Considerations**
- **Token Expiry**: Use short-lived access tokens to minimize the risk of token misuse.
- **Refresh Tokens**: Store refresh tokens securely (e.g., in HTTP-only cookies) and implement mechanisms to revoke compromised tokens.
- **Signature Verification**: Always verify the JWT signature to ensure the token hasn’t been tampered with.
- **HTTPS**: Use HTTPS to encrypt data transmitted between the client and server.

## **Common Use Cases**
- **Single Sign-On (SSO)**: JWTs are used to authenticate users across multiple applications.
- **Microservices**: JWTs enable secure communication between microservices in a distributed system.
- **Mobile and SPA Authentication**: JWTs are ideal for stateless authentication in mobile apps and single-page applications (SPAs).

---

## **Example Code Snippets**
1. **Creating a JWT (Backend)**:
```javascript
const jwt = require('jsonwebtoken');

const accessToken = jwt.sign(
  { userId: 123, email: 'user@example.com' },
  process.env.ACCESS_TOKEN_SECRET,
  { expiresIn: '15m' }
);

const refreshToken = jwt.sign(
  { userId: 123 },
  process.env.REFRESH_TOKEN_SECRET,
  { expiresIn: '7d' }
);
```

2. **Verifying a JWT (Backend)**:
```javascript
const decoded = jwt.verify(accessToken, process.env.ACCESS_TOKEN_SECRET);
console.log(decoded.userId); // Output: 123
```

3. **Sending JWT in Authorization Header (Frontend)**:
```javascript
fetch('/protected', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${accessToken}`
  }
});
```
