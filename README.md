# pocket-crypt

A single-file, zero-dependency tool for encrypting and decrypting text locally in your browser. Nothing is sent over the network — all cryptography runs on your device using the native Web Crypto API.

## Features

- **Fully offline** — no network requests, no data leaves your device. Works with your browser completely disconnected.
- **Zero dependencies** — one self-contained HTML file. No build step, no libraries. Just open it in a browser.
- **Strong encryption** — PBKDF2 (600,000 iterations, SHA-256) for key derivation and AES-256-GCM for authenticated encryption.
- **Tamper detection** — AES-GCM verifies integrity, so a modified ciphertext fails to decrypt instead of returning garbage.
- **Broad browser support** — runs in any modern browser, with a fallback for older Safari.

## How it works

Each encryption:

1. Generates a random 16-byte salt and 12-byte IV.
2. Derives a 256-bit AES key from the password and salt via PBKDF2-HMAC-SHA256 (600,000 iterations).
3. Encrypts the UTF-8 plaintext with AES-256-GCM.
4. Concatenates `salt || iv || ciphertext` (the ciphertext already includes the 128-bit GCM authentication tag) and Base64-encodes the result.

A fresh random salt and IV are used every time, so encrypting the same text twice produces different output.

### Ciphertext format

```
Base64( salt[16] || iv[12] || ciphertext+tag )
```
