# encrypted

Client-side, zero-server tool for sharing secrets. Everything is a single,
self-contained HTML file: encryption/decryption happens entirely in the browser
via the Web Crypto API (AES-256-GCM + PBKDF2), and the encrypted payload lives
in the URL hash (`#v1:...`) — it never touches a server.

## Files

- `index.html` — landing page. Links to the encryptor tools.
- `v1.html` — **note** encryptor/decryptor (text). Creates/reads `#v1:` links.
- `image-v1.html` — **image** encryptor/decryptor. Creates/reads `#v1:` links.

## Versioning rule — DO NOT break shared links

The whole point of these tools is that a link, once shared, decrypts **forever**.
A share link is just a URL pointing at a specific file + hash format. If we
change how that file encrypts or how it interprets the hash, every previously
shared link that depends on the old behavior breaks — and we can't fix them,
because the data only exists inside links people already hold.

The deeper goal: a user should be able to **download `v1.html`** (or any
versioned file) and keep using it **forever**, fully offline, with no
dependency on this server or any future change we make. The file they saved is
the guarantee — it must stay a complete, self-contained, unchanging tool. That
only holds if we never alter a published version in a way that changes what it
encrypts to or decrypts; new behavior goes in a new file instead.

So the versioned files (`v1.html`, `image-v1.html`) are **frozen** for
backwards compatibility:

- **Do not change the crypto, the payload format, or the `#v1:` hash handling**
  in an existing versioned file. Bug-fix and cosmetic tweaks that don't alter
  what a link decrypts to are fine; anything that changes the decrypt contract
  is not.
- **If you need a new format or behavior, create a new file** (e.g. `v2.html`,
  `image-v2.html`) with its own hash version, and point new links at it. Leave
  the old file in place so old links keep working.

When adding a tool, mirror this: keep it self-contained and versioned in the
filename so it can be frozen later.
