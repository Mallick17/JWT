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
