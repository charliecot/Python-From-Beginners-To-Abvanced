# Python `secrets` Module — Secure Random Values

## 1. What is `secrets`?
Python's standard-library `secrets` module creates cryptographically strong random values for password-reset tokens, verification codes, session identifiers, and other security-sensitive tasks.

```python
import secrets
token = secrets.token_urlsafe(32)
print(token)
```
`32` requests 32 random bytes of input entropy; the resulting text is not necessarily 32 characters long.

## 2. Why not `random`?
`random` is intended for simulations and ordinary random choices, not values that must remain unpredictable to attackers. Use `secrets` for security tokens.

## 3. Main functions

### `token_bytes(nbytes=None)` — random bytes
```python
data = secrets.token_bytes(16)
print(data)
print(len(data))  # 16
```

### `token_hex(nbytes=None)` — bytes represented as hex text
```python
token = secrets.token_hex(16)
print(token)
print(len(token))  # 32 hex characters for 16 bytes
```
Each byte becomes two hex characters. The result is `str`, not raw bytes.

### `token_urlsafe(nbytes=None)` — URL-safe text
```python
reset_token = secrets.token_urlsafe(32)
```
Useful when a token must be included in a URL.

### `randbelow(n)` — integer from zero up to, but excluding, `n`
```python
number = secrets.randbelow(100)  # 0 through 99
```

### `choice(sequence)` — secure selection
```python
import string
alphabet = string.ascii_letters + string.digits
code = "".join(secrets.choice(alphabet) for _ in range(20))
```

### `randbits(k)` — integer containing `k` random bits
```python
value = secrets.randbits(128)
```

## 4. Password-reset token example
```python
import secrets

def create_reset_token() -> str:
    return secrets.token_urlsafe(32)

token = create_reset_token()
print(token)
```
This generates a token, not a complete reset system. A real service should associate the token with a user, store a hash of it where practical, expire it, make it single-use, invalidate it after use, avoid logging it, use HTTPS, and rate-limit requests and attempts.

## 5. Six-digit one-time code
```python
def generate_otp() -> str:
    # A string preserves leading zeroes.
    return f"{secrets.randbelow(1_000_000):06d}"

print(generate_otp())  # e.g. '004291'
```
A six-digit code has only one million possibilities. Use short expiry, one-time use, attempt limits, and rate limiting.

## 6. `compare_digest()`
Use it when comparing secret values such as tokens:
```python
provided = "example-token"
expected = "example-token"

if secrets.compare_digest(provided, expected):
    print("Tokens match")
else:
    print("Invalid token")
```
It helps avoid content-dependent short-circuit comparison behavior, but does not replace secure storage, expiration, or authorization checks.

## 7. Choosing a representation
| Function | Returns | Common use |
|---|---|---|
| `token_bytes()` | `bytes` | APIs expecting raw bytes |
| `token_hex()` | `str` | Text representation using hex |
| `token_urlsafe()` | `str` | Tokens in URLs |
| `randbelow()` | `int` | Bounded random integer |
| `choice()` | selected item | Secure random selection |
| `randbits()` | `int` | Random integer with specified bit count |

## 8. Best practices
- Never use `random` to create authentication tokens.
- Do not hard-code production secrets in source code.
- Never log live reset tokens, API keys, or session tokens.
- Do not reuse one-time tokens.
- Set expiry and rate limits.
- `secrets` generates random values; it is not an encryption module.
- Use a reputable cryptographic library for encryption and key management.

## 9. Practice
1. Generate 16 random bytes and show them as bytes and hex.
2. Generate a URL-safe token.
3. Generate a six-digit OTP as a string.
4. Choose an item with `secrets.choice()`.
5. Compare tokens using `secrets.compare_digest()`.

**Key idea:** `secrets` is the high-level standard-library choice for unpredictable values in security-sensitive workflows.
