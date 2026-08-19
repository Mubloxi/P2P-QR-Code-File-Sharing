# Optical P2P

> Transfer files between devices using nothing but a screen and a camera.

**Optical P2P** is a browser-based, serverless-at-the-application-level file-sharing system that transfers files through animated QR codes. One device broadcasts the file as a sequence of QR frames, while another device uses its camera to scan and reconstruct the file.

No accounts.
No file uploads.
No database.
No external transfer service.

The included Node.js server simply hosts the application on **port 3005**.

## Tech Stack

* **Node.js** — Local web server
* **JavaScript** — Application logic
* **Vue 3** — Reactive UI
* **Tailwind CSS** — Styling
* **QRCode.js** — QR code generation
* **ZBar WASM** — QR code scanning
* **Pako** — GZIP compression/decompression
* **Web Camera API** — Camera access
* **Canvas API** — QR rendering and camera processing
* **HTML5** — Application structure

## Features

* Transfer files between devices using QR codes
* No cloud storage or file upload service
* Local file compression with GZIP
* Automatic file chunking
* Adjustable QR broadcast speed
* Real-time transfer progress
* Camera-based QR scanning
* Automatic packet reconstruction
* Automatic file download after transfer
* Metadata retransmission for improved reliability
* Responsive mobile-friendly interface
* Lightweight Node.js server
* Runs locally on port **3005**

## How It Works

The sender compresses the selected file, splits it into small chunks, and converts each chunk into a QR code.

The receiving device uses its camera to scan the QR codes and rebuild the original file.

```text
SENDER
  │
  ├── Select File
  │
  ├── GZIP Compress
  │
  ├── Split Into Chunks
  │
  ├── Convert Chunks → QR Codes
  │
  └──────── Screen ────────┐
                           │
                           ▼
                        Camera
                           │
                           ▼
RECEIVER               Scan QR
  │                        │
  ├── Decode QR ◄──────────┘
  │
  ├── Collect Chunks
  │
  ├── Reassemble
  │
  ├── GZIP Decompress
  │
  └── Download File
```

## Requirements

Before running the project, install:

* **Node.js 18+**
* **npm** — included with Node.js
* A modern web browser
* A camera on the receiving device

### Check Node.js

```bash
node --version
```

You should see something similar to:

```text
v18.x.x
```

or newer.

Check npm:

```bash
npm --version
```

## Installation

Clone the repository:

```bash
git clone https://github.com/Mubloxi/P2P-QR-Code-File-Sharing.git
```

Enter the project directory:

```bash
cd P2P-QR-Code-File-Sharing
```

Install the required Node.js dependencies:

```bash
npm install
```

If you're setting the project up from scratch, the server requires:

```bash
npm install express
```

The browser-side libraries are loaded directly by `index.html`, including:

* Vue 3
* Tailwind CSS
* QRCode.js
* Pako
* ZBar WASM

These do **not** need to be separately installed with npm when using the current version of the project.

## Running the Server

Start the server with:

```bash
node server.js
```

The application will run on:

```text
http://localhost:3005
```

Open that address in your browser.

### Development

Run:

```bash
node server.js
```

Then visit:

```text
http://localhost:3005
```

Stop the server with:

```text
CTRL + C
```

## Using Another Device

If you want to transfer files between two devices on the same Wi-Fi network, run the server on one device and access it from the other.

Find the host machine's local IP address.

### Windows

```powershell
ipconfig
```

Look for the IPv4 address, for example:

```text
192.168.1.50
```

Then open on the other device:

```text
http://192.168.1.50:3005
```

For example:

```text
Sender:
http://192.168.1.50:3005

Receiver:
http://192.168.1.50:3005
```

The sender displays the QR frames on its screen while the receiver points its camera at them.

> **Note:** Camera access may be restricted when using a plain HTTP LAN address. For the most reliable camera permissions, use `localhost` or serve the application through HTTPS.

## Project Structure

```text
P2P-QR-Code-File-Sharing/
│
├── index.html
├── server.js
├── package.json
├── package-lock.json
└── README.md
```

## Server

The included `server.js` provides the local HTTP server for the application.

The default port is:

```text
3005
```

Start it with:

```bash
node server.js
```

The server does **not** process or store transferred files. File processing and QR transmission happen inside the browser.

## Dependencies

### Server-side

```text
Node.js
Express
```

Install Express with:

```bash
npm install express
```

### Client-side

The frontend currently loads these libraries through CDN:

```text
Vue 3
Tailwind CSS
QRCode.js
Pako
ZBar WASM
```

They are loaded automatically when the page opens.

## Sender

1. Open the application.
2. Select **SEND**.
3. Choose a file.
4. The file is compressed locally.
5. The file is split into QR-sized chunks.
6. Choose a broadcast speed.
7. Click **START BROADCAST**.
8. Display the QR code to the receiving device.

The sender continuously cycles through the chunks so that missed frames can be captured later.

## Receiver

1. Open the application.
2. Select **RECEIVE**.
3. Click **ACTIVATE CAMERA**.
4. Allow camera access.
5. Point the camera at the sender's QR code.
6. Keep the QR code inside the scanning area.
7. Wait for all packets to be received.
8. The original file is reconstructed and downloaded automatically.

## Broadcast Speed

The sender provides an adjustable broadcast speed.

```text
SLOWER ◄────────────────────────► FASTER
       Potato                  Beast
```

Higher speeds can make transfers faster, but the receiving camera may have a harder time capturing rapidly changing QR codes.

If packets aren't being detected reliably, try lowering the broadcast speed.

## Privacy

The project is designed to keep file processing inside the browser.

Files are:

* Read locally
* Compressed locally
* Chunked locally
* Converted into QR codes locally
* Displayed locally
* Scanned locally
* Reassembled locally
* Downloaded locally

The Node.js server is only responsible for serving the web application.

## Security

The current implementation does **not encrypt file contents**.

QR data can potentially be captured by anyone who can see the transmitting screen.

Do not use the current implementation for highly sensitive information without adding encryption.

## Limitations

Optical transfer is fundamentally slower than traditional network-based file transfer.

Performance depends on:

* Screen resolution
* Camera quality
* Camera focus
* Lighting
* Distance between devices
* QR-code size
* Broadcast speed
* File size

Large files can require a significant number of QR frames.

## Roadmap

* [ ] End-to-end encryption
* [ ] Automatic speed optimization
* [ ] Improved error correction
* [ ] Transfer pause/resume
* [ ] Drag-and-drop uploads
* [ ] Multiple-file transfers
* [ ] Folder transfers
* [ ] Better mobile camera handling
* [ ] Transfer statistics
* [ ] PWA/offline support
* [ ] Optional WebRTC transfer mode
* [ ] More efficient QR encoding

## Contributing

Contributions and improvements are welcome.

Clone the repository:

```bash
git clone https://github.com/Mubloxi/P2P-QR-Code-File-Sharing.git
cd P2P-QR-Code-File-Sharing
npm install
node server.js
```

Then open:

```text
http://localhost:3005
```

## Author

Created by **Mu_bloxi**

GitHub:

https://github.com/Mubloxi

Repository:

https://github.com/Mubloxi/P2P-QR-Code-File-Sharing

---

**Optical P2P — File transfer through light, without the cloud.**
