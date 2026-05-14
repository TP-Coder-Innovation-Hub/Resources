# API Design Reference

Guides for the four most common API communication styles. Pick the right one for your use case.

| Style | File | Best for |
|-------|------|----------|
| REST | [rest.md](rest.md) | CRUD-heavy apps, public APIs, simple resource models |
| GraphQL | [graphql.md](graphql.md) | Complex nested data, multiple clients needing different shapes |
| gRPC | [grpc.md](grpc.md) | Service-to-service communication, low-latency, high-throughput |
| WebSocket | [websocket.md](websocket.md) | Real-time chat, live dashboards, collaborative editing, live tracking |

## Quick Comparison

| Aspect | REST | GraphQL | gRPC | WebSocket |
|---|---|---|---|---|
| Direction | Request/response | Request/response | Request/response + streaming | Two-way persistent |
| Format | JSON | JSON | Protobuf (binary) | Text or binary |
| Schema | Optional (OpenAPI) | Required (SDL) | Required (.proto) | Application-defined |
| Browser support | Native | Native | Needs proxy | Native |
| Performance | Medium | Medium | High | High (low latency) |
| Learning curve | Low | Medium | Medium | Low |

## How to Choose

1. **Building a public API or simple CRUD?** → REST
2. **Need flexible data fetching with nested relationships?** → GraphQL
3. **Microservices talking to each other at high throughput?** → gRPC
4. **Need real-time two-way updates?** → WebSocket
5. **Combining patterns?** Common: REST for CRUD + WebSocket for real-time features
