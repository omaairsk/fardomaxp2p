# 🔐 Fardomax Secure Transmit

<div align="center">

![Fardomax Banner](https://img.shields.io/badge/FARDOMAX-Secure%20Transmit-00c8ff?style=for-the-badge&labelColor=020408&color=00c8ff)
![Version](https://img.shields.io/badge/version-1.0.0-00ffcc?style=flat-square&labelColor=020408)
![License](https://img.shields.io/badge/license-MIT-00c8ff?style=flat-square&labelColor=020408)
![No Server](https://img.shields.io/badge/server%20storage-ZERO-ff2d55?style=flat-square&labelColor=020408)
![Encryption](https://img.shields.io/badge/encryption-AES--256--GCM-00ffcc?style=flat-square&labelColor=020408)
![HTML](https://img.shields.io/badge/built%20with-HTML%20%2B%20JS-00c8ff?style=flat-square&labelColor=020408)

**Military-grade encrypted peer-to-peer file transfer. No servers. No limits. No accounts. Works anywhere in the world.**

[Live Demo](#) · [How It Works](fardomax-how-it-works.html) · [Security](fardomax-security.html) · [Privacy Policy](fardomax-privacy.html)

</div>

---

## ✨ What Is Fardomax Secure Transmit?

Fardomax Secure Transmit is a **fully client-side, browser-based P2P file transfer app** built in a single HTML file. Files travel **directly between browsers** — encrypted end-to-end with AES-256-GCM — without ever touching a server. No cloud storage. No registration. No file size limits.

> Drop files → Generate a secure link → Share it → Recipient connects → Transfer happens peer-to-peer. Done.

---

## 🚀 Features

| Feature | Detail |
|---|---|
| 🔒 **AES-256-GCM Encryption** | Every byte encrypted before leaving your browser. Authenticated & tamper-proof. |
| ∞ **No File Size Limit** | Files streamed in 64 KB chunks — transfer gigabytes, no restriction. |
| 🌍 **Global TURN Relay** | Works across any NAT/firewall anywhere in the world via Open Relay + Metered TURN servers. |
| 🚫 **Zero Server Storage** | Files never reach our infrastructure. Architecturally impossible. |
| 🔗 **Unique Shareable URL** | Each session gets a one-time URL with the encryption key embedded in the fragment. |
| ⚡ **Unlimited Speed** | Direct P2P transfers run at your full network speed. No server throttling. |
| 👤 **No Account Required** | Open the page. Transfer. Close. No sign-up, no login, no email. |
| 🔊 **Futuristic Sound Engine** | Web Audio API synthesized sounds for every transfer event. Mutable. |
| 🎨 **Futuristic UI** | Animated particle canvas, hexagon loader, Orbitron typography, glowing cyan aesthetic. |
| 📄 **Single HTML File** | The entire app is one `.html` file. Download and run locally with zero dependencies. |

---

## 📁 Project Files

```
fardomax/
├── fardomax-secure-transmit.html   ← Main app (the file transfer tool)
├── fardomax-privacy.html           ← Privacy Policy page
├── fardomax-security.html          ← Security White Paper page
├── fardomax-how-it-works.html      ← Technical guide & FAQ page
└── README.md                       ← This file
```

---

## 🖥️ How To Use

### Quick Start

1. **Download** `fardomax-secure-transmit.html`
2. **Open** it in Chrome, Firefox, or Edge
3. That's it — no install, no server, no build step

### Sending Files

1. Drag & drop files onto the drop zone (or click to browse)
2. Click **◈ GENERATE SECURE LINK**
3. Copy the generated link
4. Send it to your recipient via Signal, WhatsApp, iMessage, or any trusted channel
5. Keep your tab open — the link is valid only while your page is open

### Receiving Files

1. Open `fardomax-secure-transmit.html` in your browser
2. Paste the share link into the Receive panel
3. Click **◈ CONNECT & RECEIVE**
4. Wait for files to arrive, then click **⬇ DOWNLOAD**

---

## 🔐 Security Architecture

### Encryption
- **Algorithm:** AES-256-GCM (Galois/Counter Mode)
- **Key length:** 256-bit — 2²⁵⁶ possible keys
- **Key generation:** `crypto.subtle.generateKey` — OS-level CSPRNG
- **IV:** 96-bit random per chunk — prevents pattern detection
- **Authentication:** 128-bit GCM tag — tamper detection built-in

### Key Distribution (Zero-Knowledge)
The encryption key is embedded in the **URL fragment** (`#`) which browsers **never send to servers** (RFC 3986). Not even the signal server sees your key.

```
Share URL format:
https://your-domain/fardomax-secure-transmit.html#[peerID]|[64-char-hex-key]
                                                   ^^^^^^^^^^^^^^^^^^^^^^^^^
                                                   Never sent to any server
```

### Transport Security
| Layer | Protocol |
|---|---|
| Application | AES-256-GCM (your key) |
| WebRTC Data Channel | DTLS 1.2+ (mandatory by spec) |
| Signal Server | TLS 1.3 |
| TURN Relay | TLS/TCP 443 |

When TURN relay is used, data is **double-encrypted** (AES-256-GCM + DTLS). The relay sees only opaque ciphertext.

### Threat Model Summary

| Threat | Status |
|---|---|
| Network eavesdropper | ✅ **BLOCKED** — Double-encrypted ciphertext only |
| TURN relay operator | ✅ **BLOCKED** — Cannot decrypt without key in URL fragment |
| Man-in-the-middle | ✅ **BLOCKED** — AES-GCM auth tag detects any modification |
| Fardomax servers | ✅ **BLOCKED** — No file data ever reaches our infrastructure |
| Brute force key attack | ✅ **BLOCKED** — 2²⁵⁶ keys, computationally infeasible |
| Signal server logging | ⚠️ **MITIGATED** — Can see peer IDs, never file data or keys |
| URL interception | ⚠️ **User Responsibility** — Share via encrypted channels |

---

## 🌐 Network Architecture

### Connection Flow

```
SENDER BROWSER                     RECEIVER BROWSER
      │                                    │
      │──── Register Peer ID ──────────────│
      │         ↓ Signal Server            │
      │──── SDP Offer ─────────────────────│
      │──── ICE Candidates ────────────────│
      │                                    │
      │         Direct P2P (preferred)     │
      │════════════════════════════════════│
      │    AES-256-GCM + DTLS encrypted    │
      │                                    │
      │   OR via TURN relay (fallback)     │
      │──→ [TURN Server] ──────────────────│
      │    Double encrypted                │
```

### ICE Server Configuration
```javascript
// STUN servers (peer discovery)
stun:stun.l.google.com:19302
stun:stun1.l.google.com:19302

// TURN servers (relay fallback — free, globally distributed)
turn:openrelay.metered.ca:80        // Open Relay Project
turn:openrelay.metered.ca:443
turn:a.relay.metered.ca:80          // Metered.ca backup
turn:a.relay.metered.ca:443
```

---

## 🔊 Sound System

All sounds are **synthesized in real-time** using the **Web Audio API** — no external audio files required.

| Event | Sound |
|---|---|
| File dropped | Soft thud + rising sweep |
| Link generated | 3-note rising arpeggio |
| Peer connected | Digital handshake sweep |
| Transmitting (live) | Rapid high-freq data ticks + animated EQ bars |
| Receiving (live) | Softer data ticks |
| File sent | Bright double ping |
| File received | Mellow 3-note bell chime |
| All complete | 6-note success fanfare |
| Download clicked | Click + descending sweep |
| Link copied | Quick double beep |
| Error | Low descending buzz |

Toggle sound on/off using the **🔊 SOUND ON** button in the header.

---

## 🛠️ Tech Stack

| Technology | Usage |
|---|---|
| **WebRTC** (native browser) | Peer-to-peer data channels |
| **PeerJS 1.5.4** | WebRTC signaling abstraction |
| **Web Crypto API** | AES-256-GCM encryption/decryption |
| **Web Audio API** | Synthesized sound effects |
| **Canvas API** | Animated particle network background |
| **CSS Animations** | Futuristic loading screen & UI effects |
| **Google Fonts** | Orbitron, Share Tech Mono, Exo 2 |

**Zero frameworks. Zero build tools. Zero backend.**

---

## 📊 Browser Support

| Browser | Support |
|---|---|
| Chrome 90+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Firefox 85+ | ✅ Full support |
| Opera 76+ | ✅ Full support |
| Safari 14.1+ | ⚠️ Works, may be slower |
| Chrome for Android | ✅ Full support |
| Firefox for Android | ✅ Full support |
| Safari for iOS | ⚠️ Limited WebRTC |
| Internet Explorer | ❌ Not supported |

**Requirements:** WebRTC Data Channels, Web Crypto API (`crypto.subtle`), ES6+

---

## ⚙️ Self-Hosting

Since Fardomax is a single HTML file, hosting is trivial:

### GitHub Pages
```bash
git clone https://github.com/yourusername/fardomax.git
cd fardomax
# Push to main branch — GitHub Pages serves it automatically
```

### Any Static Host (Netlify, Vercel, etc.)
Just upload the `.html` files. No server configuration needed.

### Locally (no internet for signaling)
```bash
# Run a local server (Python)
python -m http.server 8080

# Or with Node.js
npx serve .

# Open in browser
http://localhost:8080/fardomax-secure-transmit.html
```

> **Note:** You still need internet access for the initial WebRTC signaling handshake (PeerJS connects to `0.peerjs.com`). Once the P2P connection is established, all file data flows locally.

### Custom PeerJS Server (fully private)
To eliminate dependency on the public PeerJS signal server, run your own:

```bash
npm install -g peer
peerjs --port 9000 --key peerjs --path /myapp
```

Then update the config in the HTML:
```javascript
const PCFG = {
  host: 'your-server.com',
  port: 9000,
  path: '/myapp',
  secure: true,
  // ...
};
```

---

## 🐛 Troubleshooting

### "Connecting..." but never connects

| Cause | Fix |
|---|---|
| Sender tab closed | Sender must keep their page open during the entire transfer |
| Wrong link | Copy the **complete** URL including everything after `#` |
| Firewall blocking WebRTC | TURN relay should bypass this — try on mobile hotspot to test |
| Browser issue | Use Chrome or Firefox for best WebRTC support |
| Both on same device | Open sender in Tab 1, receiver in Tab 2 — do NOT use same tab |

### Transfer is slow
- Direct P2P is fastest — try on the same network first
- TURN relay adds latency but works globally
- Large files (1 GB+) may take time depending on network speed

### Decryption error
- Ensure you copied the **full** share URL — the key is the long string after `|`
- Generate a new link and try again

---

## 🔒 Privacy

- **Zero files stored** — no file ever touches our servers
- **Zero analytics** — no tracking pixels, no SDKs
- **Zero cookies** — no session tracking
- **Zero accounts** — no registration
- **Zero ads** — no advertising networks

Read the full [Privacy Policy](fardomax-privacy.html).

---

## 📄 License

```
MIT License

Copyright (c) 2025 Fardomax / iamfardinsk

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
```

---

## 🙌 Contributing

Contributions welcome! Here are some areas to improve:

- [ ] Resume-on-reconnect for dropped transfers
- [ ] Multi-recipient broadcast (one sender → many)
- [ ] QR code for share link
- [ ] Optional password protection layer on top of AES key
- [ ] Transfer history (local storage only)
- [ ] File preview before download
- [ ] Custom self-hosted TURN server configuration UI

**To contribute:**
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'Add my feature'`
4. Push and open a Pull Request

---

## 👤 Author

**Fardin SK** — [@iamfardinsk](https://github.com/omaairsk)

Built with 🔐 for privacy, ⚡ for speed, and 🎨 for aesthetics.

---

<div align="center">

**Fardomax Secure Transmit** · Zero Servers · Zero Limits · Zero Compromise

</div>
