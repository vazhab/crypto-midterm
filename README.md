# Applied Cryptography – Midterm Lab Exam Submission

**Student:** Vazha Bichiashvili
**Date:** 14.04.2025

## Introduction

This repository contains the solutions, commands, code, outputs, and documentation for the Applied Cryptography Midterm Lab Exam (Weeks 1–4). The tasks cover AES encryption/decryption, ECC key generation and signing/verification, hashing with SHA-256, HMAC generation and verification, and Diffie-Hellman key exchange simulation. All operations were performed using OpenSSL CLI and Python 3 as specified.

## Tools Used

*   **OpenSSL:** Version OpenSSL 3.4.1 11 Feb 2025 (Library: OpenSSL 3.4.1 11 Feb 2025)
*   **Python:** Version Python 3.13.2
*   **Operating System:** macOS Sequoia version 15.3.2
*   **Terminal/Shell:** zsh

---

## Task 1 – AES Encryption (5 pts)

### Task 1A: Encrypt a file using AES-128-CBC (2.5 pts)

1.  **Create `secret.txt`:**
    ```bash
    echo "This file contains top secret information." > secret.txt
    ```
    *Verification:*
    ```bash
    cat secret.txt
    ```
    *Output:*
    ```
    This file contains top secret information.
    ```

<img width="960" alt="image" src="https://github.com/user-attachments/assets/c0862170-5446-4edf-8032-2d9c56138430" />


2.  **Encrypt with OpenSSL using AES-128-CBC:**
    *   Chosen Passphrase: `mySup3rS3cr3tP@ss` 
    *   Command:
        ```bash
        openssl enc -aes-128-cbc -salt -pbkdf2 -in secret.txt -out secret.enc -pass pass:mySup3rS3cr3tP@ss
        ```
    *   This command encrypts `secret.txt` into `secret.enc` using AES-128 in CBC mode. It uses a salt (stored in `secret.enc`) and PBKDF2 to derive the key from the passphrase `mySup3rS3cr3tP@ss`.

3.  **File Created:** `secret.enc` now contains the encrypted data.

### Task 1B: Decrypt secret.enc (2.5 pts)

1.  **Decrypt the file:**
    *   Command:
        ```bash
        openssl enc -d -aes-128-cbc -pbkdf2 -in secret.enc -out decrypted_secret.txt -pass pass:mySup3rS3cr3tP@ss
        ```
    *   This command decrypts `secret.enc` using the same algorithm, mode, key derivation function, and passphrase, saving the result to `decrypted_secret.txt`.

2.  **Show that it matches the original:**
    *   Display decrypted content:
        ```bash
        cat decrypted_secret.txt
        ```
    *   *Output:*
        ```
        This file contains top secret information.
        ```
    *   Compare with original using `diff`:
        ```bash
        diff secret.txt decrypted_secret.txt
        ```
    *   *Output:* (No output indicates the files are identical)

<img width="1342" alt="image" src="https://github.com/user-attachments/assets/db9e2c61-582d-430a-8777-20b69b3a07ae" />

    **Conclusion:** The `diff` command produced no output, confirming the decrypted content in `decrypted_secret.txt` exactly matches the original `secret.txt`.

---

## Task 2 – ECC Signature Verification (4.5 pts)

Using OpenSSL with the `prime256v1` curve.

### Task 2A: Generate ECC keys (1.5 pts)

1.  **Generate ECC Private Key (`prime256v1`):**
    ```bash
    openssl ecparam -name prime256v1 -genkey -noout -out ecc_private.pem
    ```
    *   This creates the private key file `ecc_private.pem`.

2.  **Extract Public Key from Private Key:**
    ```bash
    openssl ec -in ecc_private.pem -pubout -out ecc_public.pem
    ```
    *   This extracts the public key from `ecc_private.pem` and saves it to `ecc_public.pem`.
    
<img width="1027" alt="image" src="https://github.com/user-attachments/assets/4070f26e-570c-42fb-bd6b-268d7208f7a5" />

### Task 2B: Sign and verify a message (3 pts)

1.  **Create `ecc.txt`:**
    ```bash
    echo "Elliptic Curves are efficient." > ecc.txt
    ```
    *Verification:*
    ```bash
    cat ecc.txt
    ```
    *Output:*
    ```
    Elliptic Curves are efficient.
    ```

2.  **Sign `ecc.txt` with the Private Key (using SHA-256 digest):**
    ```bash
    openssl dgst -sha256 -sign ecc_private.pem -out ecc.sig ecc.txt
    ```
    *   This command first hashes `ecc.txt` using SHA-256, then signs the hash using the private key `ecc_private.pem`. The resulting binary signature is saved to `ecc.sig`.

3.  **Verify the signature using the Public Key:**
    ```bash
    openssl dgst -sha256 -verify ecc_public.pem -signature ecc.sig ecc.txt
    ```
    *   *Output:*
        ```
        Verified OK
        ```
    *   **Conclusion:** The output `Verified OK` confirms that the signature `ecc.sig` is valid for the file `ecc.txt` using the public key `ecc_public.pem`. This verifies the message integrity and authenticity (proving it was signed by the holder of the corresponding private key).

<img width="1027" alt="image" src="https://github.com/user-attachments/assets/9d8d9cbf-707c-4b26-91cc-2ef97682afc8" />

---

## Task 3 – Hashing & HMAC (6 pts)

### Task 3A: SHA-256 Hash (2 pts)

1.  **Create `data.txt`:**
    ```bash
    echo "Never trust, always verify." > data.txt
    ```
    *Verification:*
    ```bash
    cat data.txt
    ```
    *Output:*
    ```
    Never trust, always verify.
    ```

2.  **Hash `data.txt` using SHA-256 (CLI Method):**
    ```bash
    openssl dgst -sha256 data.txt
    ```
    *   *Output:*
        ```
        SHA256(data.txt)= 1d7834c9c74c6f7560d4e77eb03039389e377ba5ad480970d31ebdc4205df9b3
        ```

3.  **Submitted Hash Output:** The SHA-256 hash for the original `data.txt` is `1d7834c9c74c6f7560d4e77eb03039389e377ba5ad480970d31ebdc4205df9b3`.

<img width="818" alt="image" src="https://github.com/user-attachments/assets/bc3ee8a8-f331-444e-975d-e7a832e0d0d6" />

### Task 3B: HMAC using SHA-256 (2 pts)

1.  **Key:** `secretkey123`
2.  **Create HMAC-SHA256 for `data.txt` (CLI Method):**
    ```bash
    openssl dgst -sha256 -hmac "secretkey123" data.txt
    ```
    *   *Output:*
        ```
        HMAC-SHA256(data.txt)= e550e820ecb5c998a1dadf897085dd60c059c3347d26335d7a6fff85d4d3416a
        ```

3.  **Submitted HMAC Output:** The HMAC-SHA256 for the original `data.txt` using key `secretkey123` is `e550e820ecb5c998a1dadf897085dd60c059c3347d26335d7a6fff85d4d3416a`.

    <img width="856" alt="image" src="https://github.com/user-attachments/assets/a69ab0c4-c97d-467e-a3d7-6522c27ecff8" />

### Task 3C: Integrity Check (2 pts)

1.  **Change one letter in `data.txt`:** (Changed 'v' to 'V' in 'verify')
    ```bash
    echo "Never trust, always Verify." > data.txt
    ```
    *Verification of change:*
    ```bash
    cat data.txt
    ```
    *Output:*
    ```
    Never trust, always Verify.
    ```

2.  **Recompute HMAC using the *same* key and method:**
    ```bash
    openssl dgst -sha256 -hmac "secretkey123" data.txt
    ```
    *   *Output:*
        ```
        HMAC-SHA256(data.txt)= 34ca1f6eec518454f201a04e53c724d0661d3853aee4c21900e8a1cb81695af8
        ```
   <img width="853" alt="image" src="https://github.com/user-attachments/assets/cb421208-b407-4130-a8cf-4e407f393a2c" />

3.  **Explanation:**
    *   **What happens:** The newly computed HMAC value (`34ca1f6eec518454f201a04e53c724d0661d3853aee4c21900e8a1cb81695af8`) is completely **different** from the original HMAC value (`e550e820ecb5c998a1dadf897085dd60c059c3347d26335d7a6fff85d4d3416a`) calculated in Task 3B.
    *   **Why HMAC is important:** HMAC (Hash-based Message Authentication Code) provides both **data integrity** and **data authentication**.
        *   **Integrity:** Like a simple hash, any change to the message results in a different HMAC value. This allows the receiver to detect if the message has been tampered with since the HMAC was generated. Our experiment shows that even a single character change drastically alters the output.
        *   **Authentication:** Unlike a simple hash (which anyone can compute), generating the *correct* HMAC requires knowledge of the shared secret key (`secretkey123`). If the receiver computes the same HMAC value using the shared secret key, they can be confident that the message originated from someone who possesses the key and that the message wasn't altered by an attacker who doesn't know the key.

---

## Task 4 – Diffie-Hellman Key Exchange (4.5 pts)

### Task 4A: Simulate DH Key Exchange (2 pts)

Simulation using OpenSSL CLI.

1.  **Generate DH Parameters (Shared Publicly):**
    ```bash
    openssl dhparam -out dh_params.pem 2048
    ```
    *   This command generates Diffie-Hellman parameters (a large prime `p` and a generator `g`, 2048 bits long) and saves them to `dh_params.pem`. These parameters are not secret and must be agreed upon by both parties.

2.  **Alice Generates Her Key Pair:**
    *   Generate private/public key:
        ```bash
        openssl genpkey -paramfile dh_params.pem -out alice_private.pem
        ```
    *   Extract Alice's public key (to be sent to Bob):
        ```bash
        openssl pkey -in alice_private.pem -pubout -out alice_public.pem
        ```
    *   **Alice's Public Key:** Content of `alice_public.pem`.
        ```bash
        cat alice_public.pem
        ```
       <img width="606" alt="image" src="https://github.com/user-attachments/assets/3944ad9b-7967-49fc-aa99-e9876da3b723" />

3.  **Bob Generates His Key Pair:**
    *   Generate private/public key (using the *same* parameters):
        ```bash
        openssl genpkey -paramfile dh_params.pem -out bob_private.pem
        ```
    *   Extract Bob's public key (to be sent to Alice):
        ```bash
        openssl pkey -in bob_private.pem -pubout -out bob_public.pem
        ```
    *   **Bob's Public Key:** Content of `bob_public.pem`.
        ```bash
        cat bob_public.pem
        ```
        *Output:*
        
      <img width="600" alt="image" src="https://github.com/user-attachments/assets/5c835661-5afd-48cb-97d5-f94168160786" />

4.  **Alice Derives the Shared Secret:** (Using her private key and Bob's public key)
    ```bash
    openssl pkeyutl -derive -inkey alice_private.pem -peerkey bob_public.pem -out alice_secret.bin
    ```
    *   This computes the shared secret `S` using Alice's private key and Bob's public key, saving the raw binary secret to `alice_secret.bin`.

5.  **Bob Derives the Shared Secret:** (Using his private key and Alice's public key)
    ```bash
    openssl pkeyutl -derive -inkey bob_private.pem -peerkey alice_public.pem -out bob_secret.bin
    ```
    *   This computes the shared secret `S` using Bob's private key and Alice's public key, saving the raw binary secret to `bob_secret.bin`.

6.  **Verify Shared Secrets are Identical:** We compare the derived binary secrets. Hashing them is a good way to check equality without displaying the raw binary data.
    ```bash
    sha256sum alice_secret.bin bob_secret.bin
    ```

<img width="1450" alt="Screenshot 2025-04-14 at 10 36 10 PM" src="https://github.com/user-attachments/assets/9830f440-595a-4901-9f02-96e8d65678e8" />

    *   **Conclusion:** The `sha256sum` command shows the *same hash value* for both `alice_secret.bin` and `bob_secret.bin`. This confirms that Alice and Bob independently derived the **identical shared secret key**, successfully completing the Diffie-Hellman key exchange.

### Task 4B: Real-Life Application (2.5 pts)

Diffie-Hellman key exchange is a cornerstone of modern secure communication, enabling two parties to establish a shared secret over an insecure channel without prior arrangement. Its practical applications are widespread:

*   **TLS/SSL Handshake:** Perhaps the most common use is within the Transport Layer Security (TLS) protocol that secures HTTPS web traffic. When a browser connects to a secure server, Diffie-Hellman (often the more efficient Elliptic Curve variant, ECDH) is used during the handshake phase. This allows the browser and server to agree on a symmetric session key (e.g., for AES) that will encrypt the actual application data (web pages, API calls). An eavesdropper monitoring the handshake cannot compute this shared secret.

*   **Secure Messaging (e.g., Signal Protocol):** End-to-end encrypted messaging applications like Signal, WhatsApp, and others rely heavily on the Signal Protocol, which utilizes ECDH for establishing secure sessions between users. This ensures that only the intended recipients can read messages. Furthermore, its use often enables **Perfect Forward Secrecy (PFS)**. This means that even if a long-term private key of a user is compromised later, the secrecy of past session keys (and thus past messages) derived via DH is maintained, as those session keys are ephemeral and not derivable from the long-term keys alone.

**Importance:** Diffie-Hellman's primary importance lies in solving the fundamental problem of **key distribution** in cryptography. It allows secure key agreement without needing a pre-shared secret or a physically secure channel, making it essential for initiating secure communications over public networks like the internet. It provides a foundation for confidentiality by enabling the use of efficient symmetric encryption algorithms with securely established keys.

---

## Summary of Generated Files

*   `secret.txt` (Original text file for Task 1)
*   `secret.enc` (AES encrypted file)
*   `decrypted_secret.txt` (Decrypted text file)
*   `ecc_private.pem` (ECC private key file)
*   `ecc_public.pem` (ECC public key file)
*   `ecc.txt` (Text file for ECC signing)
*   `ecc.sig` (ECC signature file)
*   `data.txt` (Text file for Hashing/HMAC, content changes during Task 3C)
*   `calculate_sha256.py` (Optional Python script for Task 3A)
*   `calculate_hmac.py` (Optional Python script for Task 3B/3C)
*   `dh_params.pem` (Diffie-Hellman parameters)
*   `alice_private.pem` (Alice's DH private key)
*   `alice_public.pem` (Alice's DH public key)
*   `bob_private.pem` (Bob's DH private key)
*   `bob_public.pem` (Bob's DH public key)
*   `alice_secret.bin` (Alice's derived shared secret)
*   `bob_secret.bin` (Bob's derived shared secret)
*   `README.md` (This documentation file)

---
**End of Submission**
