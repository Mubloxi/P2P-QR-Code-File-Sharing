# Optical P2P

> Transfer files between devices using nothing but a screen and a camera.

**Optical P2P** is a browser-based, serverless file-sharing system that transfers files through animated QR codes. One device broadcasts the file as a sequence of QR frames, while another device uses its camera to scan and reconstruct the file.

No accounts.
No uploads.
No backend.
No network connection required between the devices.

## Tech Stack

* **HTML5** — Application structure
* **CSS / Tailwind CSS** — UI and styling
* **JavaScript** — Core application logic
* **Vue 3** — Reactive UI and state management
* **QRCode.js** — QR code generation
* **ZBar WASM** — QR code scanning and decoding
* **Pako** — GZIP compression and decompression
* **Web Camera API** — Camera access and video capture
* **Canvas API** — QR rendering and camera frame processing

## How It Works

Optical P2P converts a file into compressed chunks and broadcasts those chunks as QR codes.

```text
          SENDER                              RECEIVER
             │                                    │
             │ Select File                        │
             ▼                                    │
        GZIP Compression                          │
             │                                    │
             ▼                                    │
        Split into Chunks                         │
             │                                    │
             ▼                                    │
       Generate QR Frames                         │
             │                                    │
             │     Screen → Camera                │
             ├───────────────────────────────────►│
             │                                    │
             │                              Scan QR Frames
             │                                    │
             │                              Decode Chunks
             │                                    │
             │                              Reassemble File
             │                                    │
             │                              Decompress
             │                                    │
             │                              Download File
```

Each QR frame contains either:

* **Metadata** — filename and total number of chunks
* **File data** — chunk index, total chunks, and Base64 encoded data

Metadata is periodically rebroadcast during transmission so the receiver can recover even if it starts scanning after the initial frame.

## Features

### File Transfer

* Transfer files directly between devices
* No server or database required
* Works entirely inside the browser
* Automatic file compression using GZIP
* Chunk-based transmission

### QR Broadcasting

* Animated QR-code transmission
* Adjustable broadcast speed
* Configurable frame delay
* Automatic metadata rebroadcasting
* Real-time transfer progress

### Camera Receiver

* Uses the device camera to scan QR frames
* Rear-camera preference on mobile devices
* Real-time scanning with ZBar WebAssembly
* Duplicate chunk protection
* Automatic file reconstruction

### User Interface

* Minimal monochrome interface
* Sender / Receiver mode switching
* Live transmission status
* Broadcast progress
* Packet reception progress
* Camera scanning reticle
* Responsive layout

## Speed Control

The sender includes an adjustable broadcast-speed slider.

The transmission speed controls how quickly QR frames change:

```text
Slower  ◄────────────────────────►  Faster

More reliable                         Faster
Better for cameras                    Requires better focus
```

Faster speeds can improve transfer time, but slower speeds may work better with cameras that have difficulty focusing or capturing rapidly changing QR codes.

## Requirements

A modern browser with support for:

* JavaScript
* WebAssembly
* Canvas API
* `navigator.mediaDevices.getUserMedia()`
* Camera access for receiving files

Recommended browsers:

* Chrome
* Edge
* Firefox
* Safari

For camera access, browsers generally require the page to be served from **HTTPS** or `localhost`.

## Running Locally

Because the project is completely client-side, you don't need Node.js, Python, a database, or a backend.

You can simply serve the directory with any static HTTP server.

### Python

```bash
python -m http.server 3005
```

Then open:

```text
http://localhost:3005
```

### Node.js

If you have Node.js installed:

```bash
npx serve .
```

## Usage

### Sending a File

1. Open Optical P2P on the sending device.
2. Select **SEND**.
3. Choose a file.
4. Wait for the file to be compressed and prepared.
5. Adjust the broadcast speed if necessary.
6. Click **START BROADCAST**.
7. Display the QR code to the receiving device.

### Receiving a File

1. Open Optical P2P on the receiving device.
2. Select **RECEIVE**.
3. Click **ACTIVATE CAMERA**.
4. Allow camera permissions.
5. Point the camera at the sender's QR code.
6. Keep the QR code inside the scanning area.
7. Wait until all packets are received.
8. The reconstructed file will automatically download.

## Project Structure

```text
Optical-P2P/
│
├── index.html
└── README.md
```

The current version is intentionally lightweight and can run as a single HTML file.

## Architecture

Optical P2P does not use a traditional client/server architecture.

```text
┌──────────────┐
│    Sender    │
│              │
│ File         │
│     ↓        │
│ GZIP         │
│     ↓        │
│ Chunking     │
│     ↓        │
│ QR Generator │
└──────┬───────┘
       │
       │ Optical Transmission
       │
       ▼
┌──────────────┐
│   Receiver   │
│              │
│ Camera       │
│     ↓        │
│ ZBar WASM    │
│     ↓        │
│ Decode       │
│     ↓        │
│ Reassemble   │
│     ↓        │
│ GZIP Inflate │
│     ↓        │
│ Download     │
└──────────────┘
```

This means the actual file data does not need to pass through an internet server.

## Privacy

Optical P2P is designed around local, direct transfer.

Files are:

* Compressed locally
* Split locally
* Encoded locally
* Displayed locally
* Scanned locally
* Reassembled locally
* Downloaded locally

There is no project backend responsible for storing or forwarding your files.

> Your screen and camera act as the communication channel.

## Limitations

Because the transfer happens through QR codes and a camera, performance depends heavily on the devices involved.

Potential limitations include:

* Large files can require many QR frames
* Very high broadcast speeds may be difficult for cameras to capture
* Poor lighting can reduce scanning reliability
* Camera focus affects transfer performance
* QR codes have limited data capacity per frame
* Lost frames must be encountered again during the broadcast cycle

The current implementation repeatedly cycles through the file chunks, allowing the receiver to collect missing packets without requiring a network connection.

## Security

Optical P2P does not provide encryption in its current implementation.

The transfer should therefore be considered **unencrypted**.

Anyone who can visually capture the transmitted QR codes could potentially reconstruct the transferred data.

For sensitive files, additional encryption should be implemented before transmission.

## Roadmap

Potential future improvements:

* [ ] End-to-end encryption
* [ ] Automatic transfer speed optimization
* [ ] Larger and more efficient QR payloads
* [ ] Error correction
* [ ] Transfer pause / resume
* [ ] Multiple file support
* [ ] Folder transfers
* [ ] Transfer statistics
* [ ] Better mobile camera optimization
* [ ] Drag-and-drop file selection
* [ ] PWA support
* [ ] Offline installation
* [ ] WebRTC fallback for network-connected devices

## Contributing

Contributions, ideas, and improvements are welcome.

```bash
git clone https://github.com/Mubloxi/P2P-QR-Code-File-Sharing.git
cd P2P-QR-Code-File-Sharing
```

Make your changes, test them in a modern browser, and open a pull request.

## License

Add your preferred license here.

## Author

Created by **Mu_bloxi**

* GitHub: https://github.com/Mubloxi
* Project: https://github.com/Mubloxi/P2P-QR-Code-File-Sharing

---

**Optical P2P — File transfer through light, without the cloud.**
