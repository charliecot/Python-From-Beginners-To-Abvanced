# Python `os.urandom()` — Random Bytes

## 1. What is `os.urandom()`?
`os.urandom(n)` returns `n` bytes from an operating-system source suitable for cryptographic use.

```python
import os

data = os.urandom(16)
print(data)
print(len(data))  # 16
```
It returns a `bytes` object, not a password, hex string, or encryption object.

## 2. Understand the number
`os.urandom(16)` requests 16 bytes.
- 1 byte = 8 bits.
- 16 bytes = 128 bits.
- 32 bytes = 256 bits.

A byte can contain any value from 0 to 255, including values that are not printable text.

## 3. Representing random bytes

### Raw bytes
```python
data = os.urandom(8)
print(data)
```
Python may display escapes such as `\\x91`; that is the representation of the byte values.

### Hex text
```python
data = os.urandom(8)
hex_text = data.hex()
print(hex_text)
print(len(hex_text))  # 16 characters
```
Convert back:
```python
data_again = bytes.fromhex(hex_text)
```
`hex_text` is a `str`; `data_again` is `bytes`.

### URL-safe Base64
```python
import base64

data = os.urandom(16)
encoded_bytes = base64.urlsafe_b64encode(data)
print(encoded_bytes)
print(encoded_bytes.decode("ascii"))
```
Base64 is encoding, not encryption. Anyone with the encoded value can decode it.

## 4. `os.urandom()` compared with `secrets`
For application-level tokens, `secrets` usually communicates intent more clearly:
```python
import secrets
token = secrets.token_urlsafe(32)
```
Use `os.urandom()` when you specifically need a chosen number of random bytes:
```python
import os
random_bytes = os.urandom(32)
```

| Task | Recommended starting point |
|---|---|
| Password-reset token | `secrets.token_urlsafe()` |
| Random one-time code | `secrets.randbelow()` |
| Secure random selection | `secrets.choice()` |
| Exact number of random bytes | `os.urandom(n)` or `secrets.token_bytes(n)` |

## 5. Does `os.urandom(32)` create an AES key?
It creates 32 random bytes, the key length used by AES-256, but not a complete encryption system.

```python
key = os.urandom(32)
print(len(key))  # 32 bytes
```
For encryption, use a reputable cryptographic library and an authenticated encryption mode such as AES-GCM or ChaCha20-Poly1305. Follow the library's nonce and authentication-tag requirements. Never reuse a nonce with the same key in modes such as AES-GCM. Protect keys at rest.

## 6. Random bytes are not automatically text
This may fail:
```python
data = os.urandom(16)
text = data.decode("utf-8")  # May raise UnicodeDecodeError
```
Random bytes are not guaranteed to be valid UTF-8. If you need a text representation, use:
```python
hex_text = data.hex()
data_again = bytes.fromhex(hex_text)
```
Use `.decode()` when bytes actually represent text in the chosen encoding.

## 7. Common mistakes
- Treating random bytes as a normal string.
- Decoding arbitrary random bytes directly as UTF-8.
- Believing hex or Base64 is encryption.
- Assuming random key generation replaces secure key storage.
- Inventing your own cryptographic algorithm.

## 8. Practice
1. Generate 4, 16, and 32 random bytes and print their lengths.
2. Convert bytes to hex and back.
3. Convert bytes to URL-safe Base64.
4. Explain why arbitrary random bytes may fail UTF-8 decoding.
5. Explain the difference between random bytes, encoding, hashing, and encryption.

**Key idea:** `os.urandom(n)` gives you `n` random bytes. You decide whether the next step needs bytes, text encoding, or a cryptographic library.
