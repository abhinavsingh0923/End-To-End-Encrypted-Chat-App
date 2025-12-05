# End-To-End Encrypted Chat App

A real-time, privacy-focused chat application built with React Native and Node.js that implements true end-to-end encryption using Elliptic Curve Diffie-Hellman (ECDH) key exchange and AES-128-GCM encryption.

## 🔐 Features

- **True End-to-End Encryption**: Messages are encrypted on the sender's device and can only be decrypted by the recipient
- **Real-time Communication**: Instant message delivery using WebSocket (Socket.IO)
- **Room-based Matching**: Connect with people who share similar interests
- **Anonymous Chat**: Optional anonymous chatting without revealing identity
- **Typing Indicators**: See when your chat partner is typing
- **Auto-reconnection**: Automatically waits for a new partner if disconnected
- **Cross-platform**: Works on both iOS and Android devices

## 🏗️ Architecture

The application consists of two main components:

### Client (React Native)
- **Framework**: React Native 0.70.1
- **Navigation**: React Navigation
- **Encryption**: Native crypto modules (ECDH + AES-128-GCM)
- **Real-time**: Socket.IO Client

### Server (Node.js)
- **Runtime**: Node.js with ES Modules
- **Framework**: Express
- **Real-time**: Socket.IO Server
- **Matching Logic**: Interest-based room pairing

## 🔒 Encryption Details

### Key Exchange (ECDH)
- Uses `secp256k1` elliptic curve
- Each client generates a public-private key pair
- Public keys are exchanged via the server (server never sees private keys)
- Shared secret is computed independently on both devices

### Message Encryption (AES-128-GCM)
- **Algorithm**: AES-128 in Galois/Counter Mode (GCM)
- **Key**: First 16 bytes of the ECDH shared secret
- **IV**: Fixed initialization vector (for demonstration; should be random in production)
- **Authentication**: AEAD with authentication tags to prevent tampering

## 📋 Prerequisites

- Node.js (v14 or higher)
- npm or Yarn
- React Native development environment set up
  - For iOS: macOS with Xcode
  - For Android: Android Studio with Android SDK

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/abhinavsingh0923/End-To-End-Encrypted-Chat-App.git
cd End-To-End-Encrypted-Chat-App
```

### 2. Server Setup

```bash
cd server
npm install
# or
yarn install

# Start the server
npm start
# or
yarn start
```

The server will start on port 5000 by default.

### 3. Client Setup

```bash
cd e2ee
npm install
# or
yarn install

# Run nodeify to set up crypto polyfills
npm run nodeify
# or
yarn nodeify
```

### 4. Configure Server URL

Before running the client, update the server URL in `e2ee/Screens/Chat.js`:

```javascript
// Line 79
const newSocket = io("http://YOUR_SERVER_IP:5000")
```

Replace `YOUR_SERVER_IP` with:
- `localhost` if running on emulator/simulator on the same machine
- Your computer's local IP address if running on a physical device (e.g., `192.168.0.125`)

### 5. Run the App

#### For Android:
```bash
npx react-native run-android
```

#### For iOS:
```bash
cd ios
pod install
cd ..
npx react-native run-ios
```

## 📱 How to Use

1. **Enter Name** (optional): Provide your name or stay anonymous
2. **Enter Interest**: Type a shared interest/room ID to match with others who have the same interest
3. **Tap "ENTER"**: Wait for someone with the same interest to join
4. **Start Chatting**: Once matched, exchange encrypted messages in real-time

## 🗂️ Project Structure

```
End-To-End-Encrypted-Chat-App/
├── e2ee/                      # React Native client application
│   ├── Screens/
│   │   ├── Chat.js           # Main chat interface with encryption logic
│   │   └── EnterChat.js      # Entry screen for room/name input
│   ├── Context/
│   │   └── ChatContext.js    # Global state management
│   ├── utils/
│   │   └── primes.json       # Prime numbers for crypto operations
│   ├── App.js                # Root component with navigation
│   ├── package.json          # Client dependencies
│   └── .gitignore            # Git ignore rules for client
│
└── server/                    # Node.js backend server
    ├── server.js             # Socket.IO server with matching logic
    ├── package.json          # Server dependencies
    └── .gitignore            # Git ignore rules for server
```

## 🔧 Configuration

### Server Configuration

Edit `server/server.js` to change the port:

```javascript
const port = process.env.PORT || 5000
```

### Environment Variables

You can set the following environment variables:

- `PORT`: Server port (default: 5000)

## 🛠️ Technologies Used

### Client
- React Native
- Socket.IO Client
- Node.js Crypto API (via react-native-crypto)
- React Navigation
- React Native Vector Icons

### Server
- Node.js
- Express
- Socket.IO

## 🔐 Security Considerations

### Current Implementation
This is a demonstration project. For production use, consider:

1. **Random IVs**: Use unique initialization vectors for each message
2. **Key Rotation**: Implement periodic key rotation
3. **Perfect Forward Secrecy**: Use ephemeral keys for each session
4. **Server Authentication**: Verify server identity to prevent MITM attacks
5. **Message Persistence**: Implement secure message storage if needed
6. **Rate Limiting**: Prevent abuse and DoS attacks
7. **Input Validation**: Sanitize all user inputs

### What the Server Cannot See
- ✅ Message content (encrypted)
- ✅ Private keys (never transmitted)
- ✅ Shared secrets (computed locally)

### What the Server Can See
- ⚠️ Public keys (required for exchange)
- ⚠️ Encrypted message payloads
- ⚠️ Connection metadata (IP, room ID, timing)

## 🐛 Troubleshooting

### Common Issues

**1. "Unable to connect to server"**
- Verify the server is running
- Check the server URL in `Chat.js`
- Ensure firewall allows connections on port 5000

**2. "Crypto module not found"**
- Run `npm run nodeify` or `yarn nodeify`
- Clean the build and reinstall dependencies

**3. "Partner not joining"**
- Ensure both users enter the same interest/room ID
- Check server logs for connection issues

**4. Android build failures**
- Clean the build: `cd android && ./gradlew clean`
- Rebuild: `cd .. && npx react-native run-android`

## 📄 License

MIT License - see LICENSE file for details

## 👤 Author

**Abhinav Singh**
- GitHub: [@abhinavsingh0923](https://github.com/abhinavsingh0923)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 Future Enhancements

- [ ] Group chat support
- [ ] File sharing with encryption
- [ ] Voice/Video calling
- [ ] Message persistence with local encryption
- [ ] User authentication
- [ ] Profile customization
- [ ] Read receipts
- [ ] Push notifications
- [ ] Message search
- [ ] Dark/Light theme toggle

## 📚 Learn More

- [Elliptic Curve Diffie-Hellman (ECDH)](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman)
- [AES-GCM Encryption](https://en.wikipedia.org/wiki/Galois/Counter_Mode)
- [Socket.IO Documentation](https://socket.io/docs/)
- [React Native Documentation](https://reactnative.dev/)

---

⭐ If you found this project helpful, please consider giving it a star on GitHub!
