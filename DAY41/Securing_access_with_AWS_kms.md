#  Securing Data with AWS KMS

##  Overview

This project demonstrates how to securely encrypt and decrypt sensitive data using AWS Key Management Service (KMS). It covers key creation, encryption workflows, and verification of data integrity.

---

##  Key Concepts

###  Types of Keys

#### 1. Customer Managed Keys (CMK)

* Fully controlled by the user
* Custom policies and permissions
* Manual or automatic rotation

#### 2. AWS Managed Keys

* Created and managed automatically by AWS
* Limited customization

#### 3. AWS Owned Keys

* Fully managed by AWS
* No visibility or control

---

###  Envelope Encryption

AWS KMS uses envelope encryption:

1. A **data key** is generated
2. Data is encrypted using the data key
3. The data key is encrypted using a **master key (CMK)**

✔ Benefits:

* Faster encryption
* Scalable
* Secure key handling

---

###  Logging & Auditing

All actions are logged using AWS CloudTrail:

* Tracks key usage
* Enables auditing
* Supports compliance

---

##  Lab Implementation

### Step 1: Create KMS Key

* Created a symmetric key
* Assigned alias: `nautilus-KMS-Key`
* Configured IAM permissions for usage

---

### Step 2: Encrypt File

```bash
aws kms encrypt \
  --key-id alias/nautilus-KMS-Key \
  --plaintext fileb:///root/SensitiveData.txt \
  --output text \
  --query CiphertextBlob > /root/EncryptedData.bin
```

####  Explanation (Line-by-Line)

* `aws kms encrypt`
  Calls AWS KMS encryption operation

* `--key-id alias/nautilus-KMS-Key`
  Specifies which key is used

* `--plaintext fileb:///root/SensitiveData.txt`
  Reads the file as binary input

* `--output text`
  Formats output as plain text

* `--query CiphertextBlob`
  Extracts only the encrypted content

* `> /root/EncryptedData.bin`
  Saves output to a file

✔ Output is **base64 encoded ciphertext**

---

### Step 3: Convert Base64 to Binary

```bash
base64 --decode /root/EncryptedData.bin > /root/encrypted-binary.bin
```

####  Explanation

* Converts base64 → binary format
* Required because KMS expects binary input for decryption

---

### Step 4: Decrypt File

```bash
aws kms decrypt \
  --ciphertext-blob fileb:///root/encrypted-binary.bin \
  --output text \
  --query Plaintext | base64 --decode > /root/DecryptedData.txt
```

####  Explanation (Line-by-Line)

* `aws kms decrypt`
  Calls KMS decryption operation

* `--ciphertext-blob fileb:///root/encrypted-binary.bin`
  Provides encrypted binary input

* `--output text`
  Outputs result in text format

* `--query Plaintext`
  Extracts decrypted data

* `| base64 --decode`
  Converts base64 → original content

* `> /root/DecryptedData.txt`
  Saves decrypted file

---

### Step 5: Verify Integrity

```bash
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

✔ No output = files match
✔ Confirms successful encryption and decryption

---

##  Common Issues

* Missing `fileb://` prefix
* Skipping base64 decoding
* Incorrect key or permissions
* File size exceeding KMS limit (~4 KB)

---

##  Key Takeaways

* Encryption is only part of security — key management is critical
* KMS simplifies secure key handling
* Base64 encoding/decoding is essential in CLI workflows
* Logging ensures accountability and traceability

---

##  Future Improvements

* Implement envelope encryption manually using data keys
* Integrate KMS into CI/CD pipelines (e.g., Jenkins)
* Automate key rotation and monitoring

---

##  Conclusion

This project demonstrates how AWS KMS enables secure, scalable, and auditable encryption workflows. Understanding these concepts is essential for building secure cloud applications.
