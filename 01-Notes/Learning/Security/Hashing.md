# Cryptographic Hashing

## What is Hashing?
A hash function is a mathematical algorithm that transforms input data into a fixed-size string of characters. The output (hash) serves as a unique digital fingerprint of the input.

## Main Properties
1. **Irreversible (One-way)**
   - Input → Hash is easy
   - Hash → Input is impossible
   - Example: "password123" → "ef92b778..."

2. **Consistent (Deterministic)**
   - Same input = Same hash, always
   - Different input = Different hash

3. **Fixed Length**
   - Output size never changes
   - SHA-256 = 256 bits
   - SHA-512 = 512 bits

4. **Avalanche Effect**
   - Tiny input change = Completely different hash
   - "hello" ≠ "hallo" (very different hashes)

## Common Uses
- Password storage
- Data integrity checks
- Digital signatures
- Proof-of-work systems
- Message authentication

## Security Features
- **Collision Resistant**: Can't find two inputs with same hash
- **Pre-image Resistant**: Can't work backwards from hash
- **High Performance**: Fast to compute forward
