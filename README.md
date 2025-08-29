# WebRTC Video Chat App - Backend

A Node.js backend server for a real-time video chat application using WebRTC, Socket.IO, and PeerJS.

## Features

-   **Real-time Communication**: Socket.IO for signaling and room management
-   **WebRTC Support**: PeerJS server for peer-to-peer video/audio streaming
-   **Room Management**: Create and join video chat rooms
-   **TypeScript**: Full TypeScript support for type safety
-   **CORS Enabled**: Cross-origin resource sharing for frontend integration

## Tech Stack

-   **Node.js** - Runtime environment
-   **Express.js** - Web framework
-   **Socket.IO** - Real-time bidirectional event-based communication
-   **PeerJS** - WebRTC peer-to-peer data, video, and audio calls
-   **TypeScript** - Type-safe JavaScript
-   **UUID** - Unique room ID generation

## Project Structure

```
server/
├── src/
│   ├── config/
│   │   └── serverConfig.ts     # Server configuration
│   ├── handlers/
│   │   └── roomHandler.ts      # Socket.IO room management logic
│   ├── interfaces/
│   │   └── IRoomParams.ts      # TypeScript interfaces
│   └── index.ts                # Main server file
├── dist/                       # Compiled JavaScript output
├── package.json
├── tsconfig.json
├── tslint.json
└── README.md
```

## Installation

1. **Clone the repository**

    ```bash
    git clone https://github.com/dhanyabad11/Video-Calling-App-Backend.git
    cd webrtc-video-chat-app/server
    ```

2. **Install dependencies**

    ```bash
    npm install
    ```

3. **Set up environment variables**
   Create a `.env` file in the server directory (if needed):
    ```env
    PORT=5500
    ```

## Usage

### Development Mode

Run the server in development mode with auto-reload:

```bash
npm run dev
```

### Production Mode

Build and start the server:

```bash
npm start
```

### Available Scripts

-   `npm run build` - Compile TypeScript to JavaScript
-   `npm run watch` - Watch for TypeScript changes
-   `npm start` - Build and start the server
-   `npm run dev` - Run in development mode with concurrently

## Server Endpoints

### Socket.IO Events

#### Client → Server

-   **`create-room`** - Create a new video chat room
-   **`joined-room`** - Join an existing room with `{ roomId, peerId }`
-   **`ready`** - Signal that the client is ready for peer connections

#### Server → Client

-   **`room-created`** - Emitted when a room is successfully created `{ roomId }`
-   **`user-joined`** - Emitted when a new user joins the room `{ peerId }`
-   **`get-users`** - Emitted with current room participants `{ roomId, participants }`

### PeerJS Server

-   **URL**: `ws://localhost:9000/myapp/peerjs`
-   **Port**: 9000
-   **Path**: `/myapp`
-   **Key**: `peerjs`

## API Integration

### Frontend Integration Example

```javascript
import io from "socket.io-client";
import Peer from "peerjs";

// Socket.IO connection
const socket = io("http://localhost:5500");

// PeerJS connection
const peer = new Peer(undefined, {
    host: "localhost",
    port: 9000,
    path: "/myapp",
});

// Create a room
socket.emit("create-room");

// Join a room
socket.emit("joined-room", { roomId, peerId });
```

## Room Management Flow

1. **Create Room**: Client emits `create-room` → Server generates unique room ID → Server emits `room-created`
2. **Join Room**: Client emits `joined-room` with roomId and peerId → Server adds peer to room → Server emits `get-users`
3. **Ready State**: Client emits `ready` → Server notifies other clients with `user-joined`

## Configuration

### Port Configuration

-   **Socket.IO Server**: Port 5500 (configurable in `serverConfig.ts`)
-   **PeerJS Server**: Port 9000 (hardcoded)

### CORS Configuration

Current CORS setup allows all origins (`*`) for development. Update for production:

```typescript
cors: {
    origin: "https://yourdomain.com",
    methods: ["GET", "POST"]
}
```

## Development

### Code Style

-   Uses TSLint for code linting
-   Max line length: 100 characters
-   Console.log statements show as warnings (acceptable for development)

### Building

The project uses TypeScript compilation:

```bash
npm run build  # Compiles src/ to dist/
```

### Debugging

Enable debug logs by checking the console outputs from the running server.

## Troubleshooting

### Common Issues

1. **Port Already in Use**

    - Check if port 5500 or 9000 is already occupied
    - Change ports in configuration if needed

2. **WebSocket Connection Failed**

    - Ensure both Socket.IO (5500) and PeerJS (9000) servers are running
    - Check firewall settings

3. **CORS Errors**
    - Update CORS configuration for your frontend domain
    - Ensure frontend is making requests to correct ports

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run linting: `npm run prebuild`
5. Submit a pull request

## License

ISC License

---

**Note**: This is the backend server. Make sure your frontend is configured to connect to:

-   Socket.IO: `http://localhost:5500`
-   PeerJS: `ws://localhost:9000/myapp/peerjs`
