# API Authentication Methods Comparison

| Method            | Description                         | Implementation                                                                       | Pros                                                                   | Cons                                                                         | Best For                                                       |
| ----------------- | ----------------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Basic Auth**    | Username:password encoded in base64 | `Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=`                                      | - Simple to implement<br>- Widely supported<br>- Built into HTTP       | - Credentials in every request<br>- Base64 not encryption<br>- No expiration | - Development<br>- Internal APIs<br>- Legacy systems           |
| **API Keys**      | Single token in header/query        | `X-API-Key: your_api_key_here` or<br>`/api/data?api_key=key`                         | - Easy to implement<br>- Usage tracking<br>- Rate limiting             | - No built-in expiration<br>- Limited security<br>- Key management overhead  | - B2B APIs<br>- Public APIs<br>- Service integration           |
| **Bearer Tokens** | Token in Authorization header       | `Authorization: Bearer <token>`                                                      | - Standard format<br>- Can expire<br>- Revocable                       | - Token management<br>- Must protect in transit<br>- Potential leakage       | - OAuth flows<br>- Mobile apps<br>- Web apps                   |
| **JWT**           | Encoded JSON payload                | `eyJhbGciOiJIUzI1NiIs...`                                                            | - Self-contained<br>- Stateless<br>- Custom claims                     | - Cannot revoke<br>- Size limits<br>- Not encrypted                          | - Microservices<br>- SPAs<br>- Distributed systems             |
| **OAuth 2.0**     | Authorization framework             | Different grant types:<br>- Authorization Code<br>- Client Credentials<br>- Password | - Industry standard<br>- Flexible flows<br>- Separation of concerns    | - Complex setup<br>- More overhead<br>- Multiple moving parts                | - Social login<br>- Third-party apps<br>- Enterprise systems   |
| **HMAC**          | Hash-based authentication           | `Authorization: hmac username:timestamp:signature`                                   | - Prevents tampering<br>- No plaintext secrets<br>- Timing-attack safe | - Complex implementation<br>- Clock sync required<br>- Higher overhead       | - Financial APIs<br>- Payment systems<br>- Critical operations |
| **mTLS**          | Mutual TLS authentication           | Client and server certificates                                                       | - Very secure<br>- Certificate-based<br>- No tokens needed             | - Complex setup<br>- Certificate management<br>- Infrastructure needs        | - B2B integrations<br>- Banking systems<br>- IoT devices       |

## Common Implementation Examples

### Basic Auth
```http
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

### API Key
```http
X-API-Key: abcdef123456
```

### Bearer Token
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### OAuth 2.0 Flow
```mermaid
sequenceDiagram
    Client->>Auth Server: Request Token
    Auth Server->>User: Authenticate
    User->>Auth Server: Grant Permission
    Auth Server->>Client: Access Token
    Client->>Resource: Use Token
```

## Security Best Practices

| Practice | Description | Priority |
|----------|-------------|----------|
| HTTPS Only | Always use TLS for API communications | High |
| Rate Limiting | Implement request limits per key/token | High |
| Key Rotation | Regularly rotate keys and tokens | Medium |
| Validation | Validate all authentication data | High |
| Logging | Log authentication attempts | Medium |
| Expiration | Set appropriate token expiration | High |

## Selection Guide

| Requirement | Recommended Method |
|-------------|-------------------|
| Public API | API Keys + Rate Limiting |
| Mobile App | OAuth 2.0 + JWT |
| Microservices | JWT or mTLS |
| B2B Integration | mTLS or HMAC |
| Third-party Access | OAuth 2.0 |
| IoT Devices | mTLS or API Keys |
