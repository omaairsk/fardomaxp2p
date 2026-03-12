🚀 Fardomax Secure Transmit

Fardomax Secure Transmit is a lightning-fast, ultra-secure, peer-to-peer (P2P) file-sharing web application. It allows two users to connect directly via a secure tunnel and transfer files of any size without relying on a central server to store the data.

Featuring a sleek, futuristic cyberpunk interface, Fardomax is built purely with standard HTML, CSS, and Vanilla JavaScript.

✨ Features

⚡ Unlimited Speed: Transfers happen directly between browsers. Your transfer speed is only limited by your internet connection, with no server bottlenecks.

♾️ No Size Limits: Data is chunked and streamed directly into memory. Send 10MB or 100GB; the app handles it effortlessly.

🔒 End-to-End Encryption (E2EE): Utilizes WebRTC's built-in Datagram Transport Layer Security (DTLS) and Secure Real-time Transport Protocol (SRTP). Your files are heavily encrypted before they even leave your device.

🔑 Secure Passkey & Link Sharing: Connect using a simple 6-character passkey or a direct encrypted URL.

🎨 Cyberpunk Aesthetic: Immersive UI with pulsating neon animations, dynamic loading sequences, and a responsive dark-mode design powered by Tailwind CSS.

🪶 Zero Frameworks: Pure Vanilla JS, HTML, and CSS. No React, Vue, or build steps required.

🛠️ Technology Stack

Frontend: HTML5, Vanilla JavaScript (ES Modules)

Styling: Tailwind CSS (via CDN)

Icons: Lucide Icons (via CDN)

P2P Protocol: WebRTC (Real-Time Communication Data Channels)

Signaling Server: Firebase (Firestore & Anonymous Auth)

🧠 How It Works

Unlike traditional file-sharing services (like Google Drive or Dropbox) where you upload a file to a server and the receiver downloads it from that server, Fardomax uses WebRTC.

Signaling (Firebase): When you host a file, Fardomax uses a tiny Firebase database to securely exchange "handshake" information (ICE candidates and Session Descriptions) with the receiver.

The Tunnel (WebRTC): Once the handshake is complete, a direct, encrypted P2P tunnel is established between the sender's browser and the receiver's browser.

The Transfer: The file is broken into tiny chunks (16KB) and beamed directly through the tunnel. The Firebase server never sees, touches, or stores your file.

🚀 Installation & Setup

Because Fardomax is a static web application, deployment is incredibly simple. However, you will need your own free Firebase account to handle the initial signaling handshake.

Step 1: Create a Firebase Project

Go to the Firebase Console and create a new project.

Enable Authentication and turn on the Anonymous sign-in provider.

Enable Firestore Database and create a database (Start in Test Mode or configure security rules to allow read/writes to the fardomax_rooms collection).

Go to Project Settings -> General -> Your Apps, and add a "Web App" (</>).

Copy your firebaseConfig object.

Step 2: Update the Code

Open index.html and locate the --- FIREBASE CONFIGURATION --- section inside the <script type="module"> tag.

Replace the sandbox configuration logic with your actual Firebase config:

// Replace this block:
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};

// With your actual Firebase config:
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};


Step 3: Run the App

Since this app uses ES Modules (<script type="module">), you cannot just double-click the index.html file to open it. You must serve it over a local HTTP server.

If you have Python installed:

python3 -m http.server 8000


Or using Node.js/npm:

npx serve .


Then navigate to http://localhost:8000 in your browser.

🌐 Deployment

You can host this single index.html file for free on any static hosting provider:

GitHub Pages

Vercel

Netlify

Cloudflare Pages

Simply push the repository to GitHub and connect it to your hosting provider of choice.

⚠️ Important Notes

Keep the Tab Open: Because this is a direct P2P connection, the sender must keep their browser tab open until the receiver finishes downloading the file. If the sender closes the tab, the transfer terminates instantly.

Strict Firewalls: Corporate networks or strict NATs may occasionally block WebRTC connections. The app uses public Google STUN servers to bypass most routers, but restrictive environments may cause the connection to fail.

📄 License

This project is open-source and available under the MIT License.
