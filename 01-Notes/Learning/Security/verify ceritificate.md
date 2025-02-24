### **Certificate Verification Cheat Sheet**

#### **1. Check the Certificate Chain (Trust Chain)**

**Goal:** Ensure the certificate is issued by a trusted Certificate Authority (CA) and the chain is unbroken.

- **Command:**
    
    bash
    
    CopyEdit
    
    `openssl s_client -connect <server>:443 -showcerts`
    
- **What to Check:**
    - Verify all intermediate and root certificates are trusted.

---

#### **2. Validate Expiration Dates**

**Goal:** Confirm the certificate is not expired.

- **Command:**
    
    bash
    
    CopyEdit
    
    `openssl x509 -in certificate.pem -noout -dates`
    
- **What to Check:**
    - Ensure `Not Before` and `Not After` dates are valid.

---

#### **3. Verify Hostname/Subject**

**Goal:** Ensure the certificate’s **Subject Alternative Name (SAN)** or **Common Name (CN)** matches the domain.

- **Command:**
    
    bash
    
    CopyEdit
    
    `openssl x509 -in certificate.pem -noout -text | grep -E "Subject:|DNS:"`
    
- **What to Check:**
    - Ensure the domain in the certificate matches the actual server hostname.

---

#### **4. Validate the Signature**

**Goal:** Ensure the certificate has not been tampered with.

- **Command:**
    
    bash
    
    CopyEdit
    
    `openssl verify -CAfile ca-bundle.pem certificate.pem`
    
- **What to Check:**
    - Ensure the verification completes successfully without errors.

---

#### **5. Check Revocation Status**

**Goal:** Ensure the certificate has not been revoked.

- **Using OCSP (Online Certificate Status Protocol):**
    
    bash
    
    CopyEdit
    
    `openssl ocsp -issuer intermediate.pem -cert certificate.pem -url http://ocsp.digicert.com`
    
- **What to Check:**
    - Look for **"good"** status in the OCSP response.

---

#### **6. Key Usage & Extensions**

**Goal:** Confirm the certificate is used for its intended purpose (e.g., **server authentication**).

- **Command:**
    
    bash
    
    CopyEdit
    
    `openssl x509 -in certificate.pem -noout -text | grep -A1 "Key Usage"`
    
- **What to Check:**
    - Ensure **Key Usage** and **Extended Key Usage** contain appropriate values (e.g., `Digital Signature`, `Key Encipherment`, `TLS Web Server Authentication`).