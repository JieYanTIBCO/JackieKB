# OpenSSL Command Cheat Sheet

## Certificate Operations

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Generate Private Key | `openssl genrsa -out private.key 2048` | Creates a 2048-bit RSA private key |
| Create CSR | `openssl req -new -key private.key -out cert.csr` | Generates Certificate Signing Request |
| Self-sign Certificate | `openssl req -x509 -new -nodes -key private.key -days 365 -out cert.pem` | Creates self-signed certificate valid for 1 year |
| View Certificate | `openssl x509 -in cert.pem -text -noout` | Displays certificate details |
| Check CSR | `openssl req -text -noout -in cert.csr` | Shows CSR contents |
| Convert PEM to DER | `openssl x509 -in cert.pem -outform der -out cert.der` | Converts certificate format |

## Key Management

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Generate EC Key | `openssl ecparam -genkey -name prime256v1 -out ec_private.key` | Creates EC private key |
| Extract Public Key | `openssl rsa -in private.key -pubout -out public.key` | Extracts public key from private key |
| Add Password | `openssl rsa -in private.key -aes256 -out encrypted.key` | Encrypts private key with password |
| Remove Password | `openssl rsa -in encrypted.key -out decrypted.key` | Removes password protection |
| Check Private Key | `openssl rsa -check -in private.key` | Verifies private key integrity |

## Encryption/Decryption

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Encrypt File | `openssl enc -aes-256-cbc -salt -in file.txt -out file.enc` | Encrypts file with AES-256-CBC |
| Decrypt File | `openssl enc -aes-256-cbc -d -in file.enc -out file.txt` | Decrypts encrypted file |
| Base64 Encode | `openssl base64 -in binary.file -out encoded.txt` | Encodes file in base64 |
| Base64 Decode | `openssl base64 -d -in encoded.txt -out binary.file` | Decodes base64 file |
| List Ciphers | `openssl enc -list` | Shows available ciphers |

## SSL/TLS Testing

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Test Connection | `openssl s_client -connect host:443` | Tests SSL/TLS connection |
| Show Certificates | `openssl s_client -connect host:443 -showcerts` | Displays certificate chain |
| Test Protocols | `openssl s_client -connect host:443 -tls1_2` | Tests specific TLS version |
| Check Expiry | `openssl s_client -connect host:443 2>/dev/null \| openssl x509 -noout -dates` | Shows certificate validity dates |
| Verify Chain | `openssl verify -CAfile chain.pem cert.pem` | Verifies certificate chain |

## Hash and Verification

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Generate MD5 | `openssl md5 file.txt` | Creates MD5 hash (not recommended for security) |
| Generate SHA256 | `openssl sha256 file.txt` | Creates SHA256 hash |
| Sign File | `openssl dgst -sha256 -sign private.key -out sig.bin file.txt` | Creates digital signature |
| Verify Signature | `openssl dgst -sha256 -verify public.key -signature sig.bin file.txt` | Verifies digital signature |

## Common Examples

### Create Self-signed Certificate

```bash
# Generate private key
openssl genrsa -out server.key 2048

# Create CSR with common attributes
openssl req -new -key server.key -out server.csr \
    -subj "/C=US/ST=State/L=City/O=Organization/CN=domain.com"

# Generate self-signed certificate
openssl x509 -req -days 365 -in server.csr \
    -signkey server.key -out server.crt
```

### Secure Website Setup

```bash
# Generate strong DH parameters
openssl dhparam -out dhparam.pem 2048

# Create private key and CSR in one command
openssl req -new -newkey rsa:2048 -nodes \
    -keyout domain.key -out domain.csr \
    -subj "/CN=domain.com"
```

### Check Remote Server

```bash
# Test SSL connection with full details
openssl s_client -connect domain.com:443 -servername domain.com \
    -showcerts -status -tlsextdebug

# Check supported ciphers
openssl s_client -connect domain.com:443 \
    -cipher 'ALL:eNULL' -tlsextdebug
```

## Certificate Revocation Checking

### Using CRL (Certificate Revocation List)

```bash
# Download CRL from distribution point
openssl crl -in crl.pem -text -noout

# Verify certificate against CRL
openssl verify -crl_check -CAfile ca.pem -CRLfile crl.pem cert.pem

# Generate CRL (for CA)
openssl ca -gencrl -out crl.pem
```

### Using OCSP (Online Certificate Status Protocol)

```bash
# Check certificate status via OCSP
openssl ocsp -issuer issuer.pem \
    -cert cert.pem \
    -url http://ocsp.domain.com \
    -resp_text

# OCSP stapling check
openssl s_client -connect domain.com:443 \
    -servername domain.com \
    -status

# Run OCSP responder (for testing)
openssl ocsp -index index.txt \
    -port 8888 \
    -rsigner ocsp.crt \
    -rkey ocsp.key \
    -CA ca.crt
```

### Common Revocation Commands

| Operation | Command | Explanation |
|-----------|---------|-------------|
| Check OCSP Status | `openssl ocsp -issuer issuer.pem -cert cert.pem -url http://ocsp.domain.com` | Checks certificate status via OCSP |
| Verify with CRL | `openssl verify -crl_check -CAfile ca.pem -CRLfile crl.pem cert.pem` | Verifies certificate against CRL |
| OCSP Stapling | `openssl s_client -connect domain.com:443 -status` | Checks OCSP stapling response |
| Download CRL | `openssl crl -in crl.pem -text -noout` | Views CRL contents |
| Extract CRL URL | `openssl x509 -in cert.pem -text -noout \| grep "CRL Distribution"` | Shows CRL distribution points |
| Check OCSP URL | `openssl x509 -in cert.pem -text -noout \| grep "OCSP"` | Shows OCSP responder URL |

## Best Practices

1. **Key Generation**
   - Use minimum 2048 bits for RSA
   - Prefer EC over RSA when possible
   - Keep private keys secure and backed up
   - Use strong passwords for key encryption

2. **Certificate Management**
   - Keep track of expiration dates
   - Maintain proper certificate chains
   - Use appropriate key usage extensions
   - Follow naming conventions

3. **Security**
   - Use strong ciphers (AES-256)
   - Avoid MD5 and SHA1
   - Keep OpenSSL updated
   - Protect private keys

4. **Troubleshooting**
   - Check cert chain order
   - Verify hostname matches
   - Confirm intermediate certs
   - Test with s_client

## Quick Quiz

Match the commands on the left with their purposes on the right:

- [ ] `openssl genrsa -out key.pem 2048` ↔ Generate CSR for domain
- [ ] `openssl req -new -key key.pem -out csr.pem` ↔ Create RSA private key
- [ ] `openssl x509 -in cert.pem -text` ↔ Test TLS connection
- [ ] `openssl s_client -connect host:443` ↔ View certificate details
- [ ] `openssl verify -CAfile chain.pem cert.pem` ↔ Check certificate chain

Common Tasks Quiz:

- [ ] Generate a 4096-bit RSA key pair
- [ ] Create a self-signed certificate valid for 2 years
- [ ] Convert a certificate from PEM to PKCS12 format
- [ ] Check SSL/TLS connection to a website
- [ ] Verify a certificate against a CA bundle
- [ ] Extract the public key from a private key
- [ ] Create CSR with SAN (Subject Alternative Names)
- [ ] Add password protection to a private key
- [ ] View detailed certificate information
- [ ] Test specific TLS version support

Advanced Operations:

- [ ] Generate Diffie-Hellman parameters
- [ ] Create a certificate chain bundle
- [ ] Sign a CSR with a CA certificate
- [ ] Revoke a certificate
- [ ] Create ECDSA keys
- [ ] Configure perfect forward secrecy
- [ ] Implement OCSP stapling
- [ ] Handle wildcard certificates
