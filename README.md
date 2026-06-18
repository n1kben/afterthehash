# afterthehash

Share secrets. Zero trust required.

**afterthehash** is a client-side, zero-server tool for sharing secrets — notes
and images. Everything is a single, self-contained HTML file: encryption and
decryption happen entirely in your browser via the Web Crypto API
(AES-256-GCM + PBKDF2), and the encrypted payload lives in the URL hash
(`#v1:...`) — it never touches a server.

## How it works

1. You enter a secret (text or an image) and a password.
2. The browser encrypts it locally and produces a link with the ciphertext in
   the URL fragment (`#v1:...`).
3. You share the link. The fragment never leaves the browser, so the server
   never sees your data.
4. The recipient opens the link, enters the password, and the browser decrypts
   it locally.

Because the fragment is client-side only, no request ever contains your
encrypted data — that's not a promise, it's how URL hashes work in browsers.

## Files

- `index.html` — landing page. Links to the encryptor tools.
- `v1.html` — **note** encryptor/decryptor (text). Creates/reads `#v1:` links.
- `image-v1.html` — **image** encryptor/decryptor. Creates/reads `#v1:` links.

## Usage

Open the tool in a browser — that's it. There is no build step and no
dependencies.

- Locally: open `v1.html` or `image-v1.html` directly, or serve the folder with
  any static server (e.g. `npx serve`).
- Hosted: <https://afterthehash.vercel.app>

Want to guarantee the behavior never changes? **Download the versioned file**
(`v1.html` / `image-v1.html`) and run it locally. Each file is completely
self-contained and works offline, forever.

## Versioning — shared links decrypt forever

A share link is just a URL pointing at a specific file + hash format. If we
changed how that file encrypts or how it interprets the hash, every previously
shared link would break — and we couldn't fix them, because the data only
exists inside links people already hold.

So the versioned files (`v1.html`, `image-v1.html`) are **frozen** for backwards
compatibility. The crypto, payload format, and `#v1:` hash handling never change
in a published file. New formats or behavior go in a **new file** (e.g.
`v2.html`, `image-v2.html`) with its own hash version, leaving old links working.

## Security

- **AES-256-GCM** for encryption (authenticated; tampering is detected).
- **PBKDF2** to derive the key from your password.
- The encrypted content lives only in the URL fragment, which browsers never
  send to a server.
- No analytics, external scripts, fonts, or tracking. Open the Network tab and
  watch: one request for the HTML file, then nothing.

The strength of the encryption depends on your password — use a strong one and
share it through a separate channel from the link.
