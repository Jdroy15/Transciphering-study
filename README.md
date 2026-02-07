# Transciphering-study

# From Hybrid Encryption to Transciphering: Lessons from My AES-RSA Implementation

**Jaydeep Dineshbhai Roy**  
ESILV - Master in Cybersecurity & Cloud Computing  
December 2024

---

## Project Context: AES-RSA Hybrid Encryption

In my steganography project, I implemented a hybrid encryption scheme combining symmetric (AES-256) and asymmetric (RSA-2048) cryptography. The architecture was designed to balance security, performance, and practicality:

1. **Data encryption:** AES-256 encrypts the actual message (fast, efficient for large data)
2. **Key protection:** RSA-2048 encrypts the AES key (secure key transmission)
3. **Scheme conversion:** The system converts from symmetric encryption (data) to asymmetric encryption (key), then back to symmetric for decryption

This architecture mirrors real-world protocols like TLS/SSL and is conceptually similar to transciphering in homomorphic encryption.

---

## Understanding Overhead: A Practical Lesson

During implementation, I directly experienced the performance and size trade-offs between encryption schemes:

### **Performance Overhead:**
- **AES encryption:** Extremely fast (~100-200 MB/s on standard hardware)
- **RSA encryption:** Significantly slower (~1-2 MB/s for the same hardware)
- **Practical impact:** Encrypting even a 256-bit AES key with RSA was noticeably slower than encrypting megabytes of data with AES

### **Size Overhead:**
- **AES ciphertext:** Same size as plaintext (plus small IV/padding)
- **RSA ciphertext:** 256 bytes for just a 32-byte AES key (8x expansion!)
- **Design decision:** This overhead forced me to use RSA only for the small key, not the bulk data

### **Key Insight:**
The fundamental trade-off is **security/functionality vs. efficiency**. RSA provides asymmetric key exchange capabilities that AES cannot, but at substantial cost. This trade-off becomes even more extreme with Homomorphic Encryption.

---

## Connection to Transciphering in Homomorphic Encryption

After studying recent work on transciphering (particularly AES transciphering for FHE), I recognize striking parallels with my hybrid encryption experience:

### **The HE Overhead Problem:**
Homomorphic Encryption schemes (like BFV, BGV, CKKS) have even more severe overhead than RSA:
- **Ciphertext expansion:** HE ciphertexts can be 1000-10000x larger than plaintexts
- **Computational cost:** Homomorphic operations are orders of magnitude slower than plaintext operations
- **Practical impact:** Transmitting HE-encrypted data from client to cloud is prohibitively expensive

### **Transciphering as a Solution:**
Just as I used AES for bulk encryption and RSA only for key protection, transciphering uses:
1. **Client side:** Fast symmetric encryption (e.g., AES) for data transmission
2. **Server side:** Convert symmetric ciphertext to HE ciphertext **without decryption**
3. **Process:** `HE.Eval(AES.Dec, HE.Enc(AES_key), AES.Enc(data)) = HE.Enc(data)`

This eliminates the need to transmit large HE ciphertexts while still enabling homomorphic computation.

### **Conceptual Parallel:**
```
My Project:          Plaintext → [AES] → AES_CT → [Transmit] → [RSA for key]
Transciphering:      Plaintext → [AES] → AES_CT → [Transmit] → [Convert to HE_CT]
```

Both approaches recognize that **different encryption schemes excel at different tasks** and intelligently combine them.

---

## Performance Optimization Lessons Learned

### **1. Minimize Expensive Operations:**
In my project, I minimized RSA operations by encrypting only the 32-byte key, not the entire message. Similarly, transciphering minimizes HE ciphertext transmission by sending lightweight AES ciphertexts instead.

### **2. Algorithm Selection Matters:**
I experimented with different AES modes (CBC, CTR, GCM) and found that mode selection impacts performance and security properties. For transciphering, choosing stream ciphers or optimizing AES S-box evaluation in the homomorphic domain is equally critical.

### **3. Measure and Iterate:**
I measured encryption/decryption times and ciphertext sizes to make informed design decisions. Transciphering research similarly focuses on measuring:
- Homomorphic evaluation depth (number of multiplications)
- Ciphertext noise growth
- Computational amortization across multiple transciphering operations

### **4. Real-World Constraints:**
My project taught me that theoretical security must be balanced with practical usability. Transciphering embodies this principle by acknowledging that pure HE, while powerful, has practical limitations that require hybrid approaches.

---

## Why I'm Motivated to Work on HE Transciphering

### **1. Natural Extension of My Experience:**
My hybrid encryption project gave me hands-on understanding of:
- Combining different cryptographic schemes
- Optimizing for performance vs. security trade-offs
- Implementing complex cryptographic protocols in Python

Transciphering is a natural next step—applying these concepts to the cutting edge of privacy-preserving computation.

### **2. Fascination with the Technical Challenge:**
The idea of evaluating AES decryption **homomorphically** (performing the entire AES algorithm on encrypted data) is technically fascinating. It requires:
- Deep understanding of both AES internals and HE schemes
- Creative optimization (reducing multiplicative depth, optimizing S-boxes)
- Rigorous performance analysis

### **3. Real-World Impact:**
Transciphering enables practical privacy-preserving cloud computing—allowing organizations to outsource computation on sensitive data without exposing it. This has applications in:
- Healthcare (computing on encrypted medical records)
- Finance (secure cloud analytics)
- Machine learning (training on encrypted data)

### **4. Research Opportunity:**
Current transciphering implementations still face challenges:
- High computational cost of homomorphic AES evaluation
- Limited amortization across multiple transciphering operations
- Trade-offs between security parameters and performance

I'm excited to contribute to ongoing research optimizing these systems.

---

## Conclusion

My AES-RSA hybrid encryption project provided practical insight into the fundamental challenge that transciphering addresses: **efficiently combining cryptographic schemes with different properties**. The experience of implementing scheme conversion, optimizing for performance, and managing overhead has prepared me to contribute to cutting-edge research in homomorphic encryption and transciphering.

I'm eager to deepen my understanding of lattice-based cryptography, implement HE schemes using libraries like Microsoft SEAL or PALISADE, and work on optimizing AES transciphering for real-world deployment.

---

**GitHub Repository:** [Link to your project]  
**Contact:** jdroy@outlook.in | [linkedin.com/in/rdeej](https://linkedin.com/in/rdeej)
