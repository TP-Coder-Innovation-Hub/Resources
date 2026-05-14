# gRPC

A high-performance RPC framework by Google. Uses Protocol Buffers and HTTP/2.

## How It Works

- Define services and messages in a `.proto` file
- Code is auto-generated for your language
- Communication uses binary format (smaller and faster than JSON)
- Built on HTTP/2 (supports streaming)

## Service Definition (Proto File)

```protobuf
syntax = "proto3";

service OrderService {
  rpc GetOrder (GetOrderRequest) returns (Order);
  rpc ListOrders (ListOrdersRequest) returns (stream Order);
  rpc CreateOrder (CreateOrderRequest) returns (Order);
  rpc StreamUpdates (StreamUpdatesRequest) returns (stream OrderUpdate);
}

message Order {
  string id = 1;
  string user_id = 2;
  double total = 3;
  repeated Item items = 4;
}

message GetOrderRequest {
  string id = 1;
}

message ListOrdersRequest {
  string user_id = 1;
  int32 limit = 2;
}

message CreateOrderRequest {
  string user_id = 1;
  repeated string item_ids = 2;
}

message Item {
  string id = 1;
  string name = 2;
  double price = 3;
}

message OrderUpdate {
  string order_id = 1;
  string status = 2;
}

message StreamUpdatesRequest {
  string user_id = 1;
}
```

## Communication Patterns

| Pattern | Description | Use case |
|---|---|---|
| **Unary** | Single request → single response | Like a normal HTTP call (get order, create order) |
| **Server streaming** | Single request → stream of responses | Live order updates, large result sets |
| **Client streaming** | Stream of requests → single response | File upload, batch data ingestion |
| **Bidirectional streaming** | Stream both ways | Chat, real-time sync |

## Key Concepts

**Protocol Buffers (protobuf):**
- Binary serialization format (smaller than JSON, faster to parse)
- Schema-defined — both client and server agree on types
- Backward compatible: add new fields without breaking old clients

**HTTP/2:**
- Multiplexed (multiple requests on one connection)
- Header compression
- Built-in TLS support

## Error Handling

gRPC uses status codes instead of HTTP status codes:

| Code | Meaning | Like HTTP |
|---|---|---|
| OK | Success | 200 |
| INVALID_ARGUMENT | Bad input | 400 |
| UNAUTHENTICATED | No auth | 401 |
| PERMISSION_DENIED | Not allowed | 403 |
| NOT_FOUND | Resource missing | 404 |
| ALREADY_EXISTS | Duplicate | 409 |
| INTERNAL | Server error | 500 |
| UNAVAILABLE | Service down | 503 |
| DEADLINE_EXCEEDED | Timeout | 504 |

## When to Use gRPC

| Good fit | Not ideal |
|---|---|
| Service-to-service communication (microservices) | Browser clients (requires gRPC-Web proxy) |
| Low-latency, high-throughput needs | Simple CRUD APIs consumed by web apps |
| Streaming data between services | Teams unfamiliar with protobuf |
| Polyglot environments (many languages) | Public-facing APIs (REST is more accessible) |

## REST vs gRPC vs GraphQL Quick Comparison

| Aspect | REST | gRPC | GraphQL |
|---|---|---|---|
| Format | JSON | Protobuf (binary) | JSON |
| Schema | OpenAPI (optional) | .proto (required) | SDL (required) |
| Streaming | No | Yes (4 patterns) | Subscriptions only |
| Browser support | Native | Needs proxy | Native |
| Performance | Medium | High | Medium |
| Learning curve | Low | Medium | Medium-High |
| Best for | Public APIs | Internal microservices | Flexible data fetching |

## Common Tools

| Tool | Purpose |
|---|---|
| `protoc` | Protocol Buffer compiler |
| grpcurl | Like curl but for gRPC |
| grpcui | Web UI for gRPC testing |
| Evans | Interactive gRPC client |
| grpc-gateway | gRPC to REST proxy |
