# WebSocket

A protocol for persistent, two-way communication between client and server over a single TCP connection.

## How It Works

1. Client sends an HTTP request with `Upgrade: websocket` header
2. Server responds with `101 Switching Protocols`
3. Connection stays open — both sides can send messages anytime
4. No more request/response cycle — just messages in both directions

```
Client                              Server
  |--- HTTP Upgrade Request ------->|
  |<-- 101 Switching Protocols -----|
  |<========= WebSocket ==========>|
  |<--- message -------------------|  (server → client)
  |------------------- message --->|  (client → server)
  |<--- message -------------------|
  |------------------- message --->|
  |<========= close ==============>|
```

## Message Format

WebSocket messages can be **text** (usually JSON) or **binary** (raw bytes).

**Text message example:**
```json
{
  "type": "chat_message",
  "data": {
    "from": "user_123",
    "text": "Hello!",
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

**Binary message:** used for real-time streaming (audio, video, game state).

## Common Patterns

### Chat Application
```
Client A sends:  { "type": "message", "to": "room_1", "text": "Hi" }
Server broadcasts to room_1 members: { "type": "message", "from": "A", "text": "Hi" }
```

### Live Notifications
```
Server pushes: { "type": "notification", "event": "order_shipped", "orderId": "456" }
```

### Real-time Data (e.g., stock prices, GPS tracking)
```
Server pushes every 1s: { "type": "price_update", "symbol": "AAPL", "price": 185.42 }
```

## Connection Lifecycle

| Event | When | Action |
|---|---|---|
| `onopen` | Connection established | Start sending/receiving |
| `onmessage` | Message received | Process the message |
| `onerror` | Error occurred | Log and attempt recovery |
| `onclose` | Connection closed | Reconnect if unexpected |

## Reconnection Strategy

Connections drop. Always implement reconnection:

```
1. Detect close (onclose event)
2. Wait with exponential backoff (1s, 2s, 4s, 8s, max 30s)
3. Attempt reconnect
4. Re-subscribe to channels/rooms after reconnect
5. Fetch missed messages (if your server supports it)
```

## Heartbeat (Keep-Alive)

Prevent idle connections from being closed by proxies/load balancers:

```
Every 30 seconds:
  Client sends: { "type": "ping" }
  Server responds: { "type": "pong" }

If no pong within 10 seconds → close and reconnect.
```

## Authentication

WebSocket doesn't support custom headers in the browser. Common approaches:

| Approach | How | Notes |
|---|---|---|
| Query param | `ws://host/ws?token=xxx` | Token appears in server logs — use with caution |
| Cookie | Send auth cookie on upgrade request | Works if client and WS share domain |
| First message | Send auth token as first WS message | Server disconnects if invalid |

## When to Use WebSocket

| Good fit | Not ideal |
|---|---|
| Real-time chat | Simple request/response APIs |
| Live dashboards | Occasional polling is sufficient |
| Multiplayer games | One-way server push (use SSE instead) |
| Collaborative editing | Low-traffic, infrequent updates |
| Live tracking (GPS, stocks) | |

## WebSocket vs Server-Sent Events (SSE) vs Polling

| Aspect | WebSocket | SSE | Long Polling |
|---|---|---|---|
| Direction | Two-way | Server → client only | Client request, server holds |
| Format | Text or binary | Text only | HTTP response |
| Connection | Persistent, full-duplex | Persistent, one-way | New request each time |
| Reconnection | Manual | Built-in | N/A |
| Browser support | All modern | All modern | All |
| Overhead | Low after handshake | Low | Higher (HTTP per request) |
| Best for | Chat, games, real-time sync | Notifications, live feeds | Legacy fallback |

## Common Libraries

| Language | Library |
|---|---|
| JavaScript (browser) | Native `WebSocket` API |
| Node.js | `ws`, `socket.io` |
| Python | `websockets`, `FastAPI WebSocket` |
| Java | `Java-WebSocket`, Spring WebSocket |
| Go | `gorilla/websocket`, `nhooyr.io/websocket` |
