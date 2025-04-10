### **REST vs. gRPC Comparison Table**

| **Feature**             | **REST**                          | **gRPC**                                                                                                     |
| ----------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Protocol**            | HTTP/1.1 or HTTP/2                | HTTP/2 (mandatory)                                                                                           |
| **Data Format**         | JSON/XML (text-based)             | Protocol Buffers (binary)                                                                                    |
| **Performance**         | Moderate (text parsing overhead)  | High (binary serialization, multiplexing)                                                                    |
| **Communication Model** | Request-Response only             | **4 modes**:  <br>1. Unary  <br>2. Server Streaming  <br>3. Client Streaming  <br>4. Bidirectional Streaming |
| **API Contract**        | Informal (OpenAPI/Swagger docs)   | Strictly defined via `.proto` files                                                                          |
| **Browser Support**     | Native (via Fetch/XMLHttpRequest) | Limited (requires gRPC-Web proxy)                                                                            |
| **Use Cases**           | Public APIs, CRUD operations      | Microservices, IoT, real-time systems                                                                        |
| **Code Generation**     | Manual/third-party tools          | Built-in (protoc compiler)                                                                                   |
| **Streaming Support**   | Limited (SSE/WebSocket needed)    | Native bidirectional streaming                                                                               |
| **Load Balancing**      | Client-side or proxy-based        | Client-side (advanced algorithms)                                                                            |
| **Security**            | HTTPS + JWT/OAuth                 | TLS encryption + interceptors                                                                                |


---

### **Key Characteristics of gRPC**

#### 1. **Protocol Buffers (Protobuf)**

- **Strong Typing**: Enforces strict data schemas via `.proto` files.  
    Example schema:
    
```protobuf
message User {
      string id = 1;
      string name = 2;
      int32 age = 3;
    }
```
    
    
- **Binary Serialization**: Reduces payload size by 70-80% compared to JSON.
    

#### 2. **HTTP/2 Foundation**

- **Multiplexing**: Multiple requests/responses over a single TCP connection.
    
- **Header Compression**: HPACK algorithm minimizes overhead.
    

#### 3. **Cross-Language Support**

- Auto-generate client/server code in 11+ languages (Go, Python, Java, C#, etc.).
    

#### 4. **Streaming Capabilities**

- **Real-time scenarios**:
    
    go
    
    复制
    
    // Bidirectional streaming example (Go)
    stream, _ := client.Chat(context.Background())
    go func() {
      for {
        msg, _ := stream.Recv() // Server messages
        fmt.Println(msg.Text)
      }
    }()
    stream.Send(&ChatMessage{Text: "Hello"})
    

#### 5. **Built-In Features**

- **Deadlines/Timeouts**: Prevent hung calls.
    
- **Interceptors**: Middleware for auth/logging.
    
    python
    
    复制
    
    # Auth interceptor example
    def auth_interceptor(metadata):
        metadata.append(('auth-token', 'secret'))
        return metadata
    
- **Load Balancing**: Integrated with service meshes (Istio, Linkerd).
    

#### 6. **Performance Metrics**

- Latency: 5-10x lower than REST in microservice-to-microservice communication.
    
- Throughput: Handles 50k+ RPS per node with minimal CPU usage.
    

---

### **When to Choose gRPC?**

- **Microservices Architecture**: Internal service-to-service communication.
    
- **Polyglot Systems**: Teams using multiple programming languages.
    
- **Low-Latency Needs**: IoT, financial trading, gaming.
    
- **Real-Time Data**: Chat apps, live dashboards.
    

### **When to Use REST?**

- **Public APIs**: Browser/mobile client compatibility.
    
- **Simple CRUD**: Basic resource management.
    
- **Legacy Systems**: Integration with older infrastructure.