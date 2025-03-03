# Encryption Cheat Sheet

## Table of Contents
- [Basic Concepts](#basic-concepts)
- [Types of Encryption](#types-of-encryption)
- [Symmetric Encryption](#symmetric-encryption)
- [Asymmetric Encryption](#asymmetric-encryption)
- [Common Algorithms](#common-algorithms)
- [Best Practices](#best-practices)
- [Code Examples](#code-examples)

## Basic Concepts

### What is Encryption?
- Process of converting plaintext into ciphertext
- Uses mathematical algorithms and keys
- Provides confidentiality and security for data

### Key Terms
- **Plaintext**: Original readable data
- **Ciphertext**: Encrypted, unreadable data
- **Key**: Secret value used in encryption/decryption
- **Algorithm**: Mathematical process for encryption/decryption
- **Salt**: Random data added to input before hashing

## Types of Encryption

### At Rest
- Data stored in databases, files, or devices
- Examples: Full disk encryption, file encryption
- Common tools: BitLocker, FileVault, VeraCrypt

### In Transit
- Data moving between systems
- Examples: HTTPS, SSL/TLS, VPN
- Protects against man-in-the-middle attacks

## Symmetric Encryption

### Characteristics
- Single key for encryption and decryption
- Faster than asymmetric encryption
- Better for large data volumes
- Requires secure key exchange

### Popular Algorithms

#### Block Ciphers
- **AES (Advanced Encryption Standard)**
  - Key sizes: 128, 192, 256 bits
  - Industry standard for symmetric encryption
  - Most widely used and secure
  

## Asymmetric Encryption

### Characteristics
- Uses key pairs (public and private)
- Slower than symmetric encryption
- Better for key exchange and digital signatures
- No need for pre-shared keys

### Popular Algorithms
- **RSA**
  - Key sizes: 2048, 4096 bits
  - Used for key exchange and digital signatures
  
- **ECC (Elliptic Curve Cryptography)**
  - Smaller key sizes
  - More efficient than RSA
  - Used in mobile and IoT devices

## Common Algorithms

### Hash Functions (One-way Encryption)
```plaintext
MD5        - 128 bits (Not secure, avoid using)
SHA-1      - 160 bits (Not secure, avoid using)
SHA-256    - 256 bits (Recommended)
SHA-512    - 512 bits (Recommended)
```

### Block Ciphers Comparison
```plaintext
AES-256    - 256-bit key (Fastest, most widely used)
Twofish    - 256-bit key (Strong security, slower than AES)
Serpent    - 256-bit key (Most conservative design, slowest)
ChaCha20   - 256-bit key (Excellent for mobile/low-power devices)
```

## Best Practices

### Key Management
1. Regular key rotation
2. Secure key storage
3. Key backup procedures
4. Access control for keys

### Algorithm Selection
1. Use standard algorithms (avoid custom)
2. Choose appropriate key sizes
3. Consider future security needs
4. Follow industry standards

### Implementation Tips
1. Use established libraries
2. Keep encryption tools updated
3. Implement proper error handling
4. Regular security audits

## Code Examples

### Node.js AES Encryption
```javascript
const crypto = require('crypto');

const encrypt = (text, key) => {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv('aes-256-gcm', key, iv);
  
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  
  return {
    encrypted,
    iv: iv.toString('hex'),
    tag: cipher.getAuthTag().toString('hex')
  };
};

const decrypt = (encrypted, key, iv, tag) => {
  const decipher = crypto.createDecipheriv('aes-256-gcm', key, Buffer.from(iv, 'hex'));
  decipher.setAuthTag(Buffer.from(tag, 'hex'));
  
  let decrypted = decipher.update(encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  
  return decrypted;
};
```

### Python RSA Example
```python
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding
from cryptography.hazmat.primitives.asymmetric import rsa

# Generate key pair
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)
public_key = private_key.public_key()

# Encrypt
ciphertext = public_key.encrypt(
    message,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)

# Decrypt
plaintext = private_key.decrypt(
    ciphertext,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)
```

### Common OpenSSL Commands
```bash
# Generate private key
openssl genrsa -out private.pem 2048

# Extract public key
openssl rsa -in private.pem -pubout -out public.pem

# Encrypt file
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc

# Decrypt file
openssl enc -d -aes-256-cbc -in file.enc -out file.txt
