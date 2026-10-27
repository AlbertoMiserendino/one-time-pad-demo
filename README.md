# One-Time Pad Demo

⚗️️ Try online at [https://albertomiserendino.github.io/one-time-pad-demo/](https://albertomiserendino.github.io/one-time-pad-demo/).

An interactive, browser-based demo of byte-by-byte XOR encryption and decryption. Explore how a message, a key, and ciphertext relate, including how Unicode text is encoded as UTF-8.
## Try the demo

The page starts with an encrypted example and an automatically generated key.

1. Select **plain text** as the message format and enter your message.
2. Click **Clear** beside the key field, then **Encrypt** to generate a fresh key and ciphertext. Alternatively, enter a key in the selected numeric format.
3. In Results, copy the ciphertext under **In hexadecimal** and paste it into the Message field.
4. Select **hex** as the message format. Keep the same key and key format.
5. Click **Decrypt**. The decoded text should match the original message, including leading and trailing spaces.
6. Before encrypting another message, clear or replace the key. **Generate new key** creates a key matching the current input's byte length; it does not encrypt the input.

## Input formats and results

Plain text is encoded as UTF-8. Numeric input accepts bytes separated by spaces or commas:

| Format | Example for `AB` | Accepted byte values |
| --- | --- | --- |
| Decimal (`dec`) | `65 66` | Integers from 0 to 255 |
| Binary (`bin`) | `01000001 01000010` | Exactly eight bits per byte |
| Hexadecimal (`hex`) | `41 42` | One or two hexadecimal digits per byte, without `0x` |

Keys use numeric formats. Changing the key format converts the existing key. Changing the message format reinterprets the field; it does not convert its contents. Decryption requires ciphertext in a numeric format.

Results show input bytes, key bytes used, and output bytes. The table shows one row per byte, with zero-based positions. A light cerulean background groups bytes belonging to a valid multibyte UTF-8 character: for example, the globe emoji occupies four bytes (`F0 9F 8C 8D`). Arbitrary byte sequences may not represent valid UTF-8; use the numeric output as the authoritative result.

## How it works

For each byte, encryption applies XOR:

```text
C = M XOR K
M = C XOR K
```

For example, message bytes `65 66` and key bytes `1 2` produce ciphertext bytes `64 64`.

A supplied key must contain at least as many bytes as the input. For an input of N bytes, both encryption and decryption use the first N key bytes. Shorter keys are rejected. Results show only the used prefix; the key field retains the full key.

The demo does not track consumed key bytes or advance to an unused portion. Encrypting a different message with the same key field reuses its prefix. Clear or replace the key first.

## Security limits

A true one-time pad requires uniformly random, secret key bytes, with a fresh byte for every message byte. Used key bytes must never encrypt another message. Reusing them exposes the XOR of the two plaintexts:

```text
C1 XOR C2 = M1 XOR M2
```

This demo generates keys with `crypto.getRandomValues()`, a browser cryptographic random generator. It does not establish the information-theoretic randomness required by a true one-time pad, provide secure key exchange, or authenticate messages. A wrong key or modified ciphertext cannot reliably be detected. Message lengths remain visible.

Calculations run in your browser; the demo has no server-side encryption service. Keys and plaintext are displayed on the page. This is an educational tool, unsuitable for sensitive data.
