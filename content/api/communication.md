## REST
_REST (Representational State Transfer)_
https://en.wikipedia.org/wiki/REST

**REST** is an architectural style for designing **networked applications**. It uses standard **HTTP methods** and **stateless** communication, typically in web APIs.

**Stateless** operations – no client context is stored on the server between requests. This makes applications more **scalable** (you can add "any" amount of instances) and **maintainable** (easy data flow).

Important: Use HTTP standart headers and struct your responses using some protocol (check [[data-structure]]).
### REST API Design Guidelines

1. **Use HTTP methods correctly**  
    `GET` (read), `POST` (create), `PUT` (replace), `PATCH` (update), `DELETE` (remove)
2. **Use nouns for endpoints, not verbs**  
	Good: `/users`  
	Bad: `/getUsers`
3. **Keep URIs plural**  
    `/books`, `/orders`, `/products`
4. **Use nested routes for relationships**  
    `/users/123/posts`
5. **Be stateless**  
    Each request should contain all needed info  
    → Improves **scalability** and **maintainability**
6. **Return proper HTTP status codes**  
    `200 OK`, `201 Created`, `400 Bad Request`, `404 Not Found`, `500 Internal Server Error`
7. **Use JSON as default format**  
    It’s lightweight, readable, and widely supported
8. **Include filtering, sorting, and pagination**  
    `/users?role=admin&sort=name&page=2&limit=10`
9. **Implement versioning**  
    `/api/v1/` – prevents breaking changes for clients
10. **Secure your API**  
    Use **authentication** (e.g., OAuth, JWT) and **authorization**
11. **Document your API**  
    Use tools like **Swagger/OpenAPI** for clear, shareable docs

### Pros & Cons
- **Pros of REST**
	1. **Simplicity** - Uses standard HTTP methods and status codes. And overall is easy to understand and use.
	2. **Scalability** - Statelessness enables easy horizontal scaling
	3. **Language & Platform Independent** - Works with any tech stack that can send HTTP requests
	4. **Caching** - GET requests can be cached to improve performance
	5. **Decoupled client-server** - Frontend and backend can evolve independently
	6. **Widespread support** - Tools, libraries, and community support are abundant
- **Cons of REST**
	1. **Over-fetching or under-fetching** - Clients may receive too much or too little data (no built-in control like GraphQL)
	2. **No built-in versioning**- Must handle API versioning manually
	3. **Statelessness trade-off** - Client must handle more logic (e.g., session state)
	4. **Lack of standards for complex operations** - For non-CRUD actions (like `/checkout`), design patterns vary. Also there is no protocol for response body.
	5. **Multiple round-trips** - To fetch related resources, multiple requests may be needed (no aggregation)
### HTTP Methods

| Method   | Description               | Example             |
| -------- | ------------------------- | ------------------- |
| `GET`    | Retrieve resource         | `GET /users/123`    |
| `POST`   | Create resource           | `POST /users`       |
| `PUT`    | Replace resource          | `PUT /users/123`    |
| `PATCH`  | Partially update resource | `PATCH /users/123`  |
| `DELETE` | Delete resource           | `DELETE /users/123` |
###  Resource Naming (URI Design)
- Use **nouns** (not verbs): `/users`, not `/getUsers`
- Use **plural**: `/books`, `/orders`
- Nesting for sub-resources: `/users/123/posts`
### Status Codes

| Code | Meaning               | Example Use Case        |
| ---- | --------------------- | ----------------------- |
| 200  | OK                    | Successful GET          |
| 201  | Created               | Successful POST         |
| 204  | No Content            | Successful DELETE       |
| 400  | Bad Request           | Invalid input           |
| 401  | Unauthorized          | Auth required           |
| 404  | Not Found             | Resource doesn’t exist  |
| 500  | Internal Server Error | Server crash, bug, etc. |
_But there is more of them._
### Anti-patterns to Avoid
- Using `POST` for everything
- Verb in URI (`/createUser`)
- Returning 200 for errors (always use proper codes)
### Good APIs examples
- Postman API
- Paypal API
### OpenAPI
https://www.openapis.org/
### TypeSpec
https://typespec.io/
## RPC
_RPC remote procedure call_
https://en.wikipedia.org/wiki/Remote_procedure_call
### What is RPC?
**Remote Procedure Call (RPC)** is a protocol that allows a program to execute a procedure (function/method) on a different address space (commonly on another computer in a network), as if it were a local procedure.
### Pros & cons
- Pros
	- Abstracts network communication
	- Makes distributed systems easier to manage
	- Supports cross-language service calls (e.g., Python client to Java server)
- Cons
	- Network failures can make local-looking calls fail
	- Versioning and compatibility can be tricky
	- Debugging is harder than local code
	- Can introduce latency
### Core Concepts

| Term              | Meaning                                                                        |
| ----------------- | ------------------------------------------------------------------------------ |
| **Client**        | The caller of the remote procedure                                             |
| **Server**        | The provider of the remote procedure                                           |
| **Stub**          | Auto-generated code that handles the communication (client-side & server-side) |
| **Serialization** | Converting data structures into a transmittable format (e.g., JSON, Protobuf)  |
| **Transport**     | Underlying communication mechanism (e.g., HTTP, TCP)                           |
### RPC Workflow
1. Client calls a local stub method.   
2. Stub serializes the request and sends it to the server.
3. Server receives, deserializes, and executes the function.
4. Result is serialized and sent back to client.
5. Client stub receives and returns result to caller.
### Example (Basic Python Pseudo-RPC)
#### Server:
```python
def add(x, y):
    return x + y
```
#### Client (RPC Call):
```python
result = rpc_call("add", args=[5, 3])
print(result)  # 8
```
> `rpc_call()` is a function that would handle the connection, send request, and receive response.
### Popular RPC Frameworks

| Framework                           | Language  | Protocol          |
| ----------------------------------- | --------- | ----------------- |
| [gRPC](https://grpc.io/)            | Multilang | HTTP/2 + Protobuf |
| JSON-RPC (check [[data-structure]]) | Any       | HTTP/TCP + JSON   |
| XML-RPC                             | Any       | HTTP + XML        |
| Apache Thrift                       | Multilang | Custom Binary     |

## MCP
_Model Context Protocol_
https://modelcontextprotocol.io/introduction
https://modelcontextprotocol.io/specification/2025-06-18

**Brief Review of MCP (Model Context Protocol):**
MCP is an open protocol designed to standardize how large language models (LLMs) connect with data, tools, and external systems. It's like a “USB-C for LLMs”—providing a universal interface to plug models into various local and remote resources. But has many drawbacks that limits production usage.
### TL;DR
- **Good for:**
    - Personal projects
    - Early-stage prototyping
    - Low-risk experiments with LLMs
    - Complementary integrations with non-strict SLA (but with wrapping anyway)
- **Not recommended for (read Drawbacks):**
    - Enterprise production environments
    - High-availability systems
    - Security- and compliance-sensitive contexts
### Key Highlights:
- **Standardized Integration**: MCP makes it easy for developers to connect LLMs to databases, files, APIs, and tools using a consistent, open protocol based on JSON-RPC 2.0 (check [[data-structure]]).
- **Modular Architecture**: It follows a client-server model where hosts (like Claude or IDEs) connect to MCP servers, each exposing specific resources or functions.
- **Rich Capabilities**: Servers can provide:
    - **Resources** (contextual data),
    - **Prompts** (predefined workflows),
    - **Tools** (executable functions),   
    - **Sampling** (agent-style behavior),
    - And more.
- **Security by Design**: MCP emphasizes user consent, data privacy, and safe execution of tools. It requires explicit authorization before accessing data or invoking actions.
- **Ecosystem-Friendly**: It supports interoperability across tools and LLM providers, with official examples, tutorials, and a growing list of integrations.
### Ideal For:
- Developers building LLM-enhanced applications
- Teams creating AI workflows that need consistent data/tool access
- Platforms that want pluggable, secure LLM extensions
### Drawbacks
#### Technical and Implementation Issues
1. **Low Reliability in Practice**
    - Despite the existence of thousands of servers (e.g., Glama.ai lists 5286), only a small fraction are actually functional.
    - Even top-rated servers (e.g., Vizro by McKinsey) suffer from 200+ open bugs.
    - The official MCP SDK also has over 200 unresolved issues.
2. **Complex and Fragile Setup**
    - Running MCP servers often involves managing hundreds of dependencies and paths (especially in containerized environments like Docker).
    - Debugging and development require specialized tools and deep knowledge, making onboarding steep for many developers.
3. **Lack of Robust Error Handling**
    - MCP lacks comprehensive error-handling mechanisms, making debugging difficult.
    - Unclear error messages are common, leading to developer frustration.
4. **Stateful Protocol via SSE**
    - The use of Server-Sent Events (SSE) for stateful communication complicates **horizontal scaling**, which is essential in enterprise-grade deployments.
5. **Not CI/CD Friendly**
    - Unlike REST APIs, MCP lacks the maturity and simplicity for quick CI/CD integration.
    - Debugging is more complex compared to traditional REST + OpenAPI-based systems.
- 6. **Not Observability Friendly**
	- Most observability is built for REST-type services, and most infrastructures lean towards this approach, so MCP poses a challenge for transparent infrastructures.
#### Architectural and Conceptual Concerns
1. **Mismatch with Enterprise Needs**
    - No built-in SLA, monitoring, or auditability out of the box.
    - High error tolerance (2–5%) is unacceptable in enterprise systems where failures can mean real revenue/data losses.
    - Not easily integrable into existing infrastructure compared to REST or gRPC. (as said before)
2. **Overpromising Product Narrative**
    - The vision ("plug and play any model with any tool and it just works") overestimates current model capabilities.
    - LLMs still struggle with unfamiliar tools and unpredictable interfaces unless carefully tuned—MCP doesn’t solve this limitation.
3. **Protocol Conceptual Fragility**
    - Encourages unrestricted tool chaining and context injection, which increases complexity and unpredictability.
    - Fails to promote modular, testable, and tightly controlled LLM subcomponents—key principles for reliable AI system design.
#### User & Developer Experience Problems
9. **Poor Developer Support Ecosystem**
    - The protocol evolves rapidly and is under-documented.
    - Complaints on Hacker News and Reddit highlight frustrations around setup, inconsistencies, and lack of clarity.
10. **Vendor Lock-In Risks**
	- While MCP is advertised as an open protocol, most working implementations (e.g., Claude Desktop) are still Anthropic-dependent.
11. **Primarily Consumer-Grade**
	- Designed more for personal experiments, desktop agents, and prototypes.
	- Consumer use cases tolerate higher error rates; enterprises cannot.
	- Real-world LLM deployments require strict control, reproducibility, and predictability—not “plug-and-pray” design.
	- MCP encourages connecting arbitrary tools to arbitrary LLMs without guarantees of reasoning quality.
## AsyncAPI
https://www.asyncapi.com/en

AsyncAPI is an **open-source specification** (like OpenAPI for REST) designed to **document and define Event-Driven Architectures (EDAs)** such as:
- Kafka
- MQTT
- WebSocket
- AMQP (e.g., RabbitMQ)
- NATS, etc.
It helps teams describe asynchronous APIs in a standardized way.
### Event-Driven Architecture (EDA)
[**EDA**](https://en.wikipedia.org/wiki/Event-driven_architecture) is a software architecture pattern where **events** drive communication between **decoupled services or components**. Instead of making direct API calls, services **emit** and **react to events** asynchronously.

**Example:**  
When a user signs up:
- Service A (User Service) emits a `user.signedup` event.
- Services B and C (Email Service, Analytics Service) **listen** to that event and act accordingly (send email, log analytics).
### Enter AsyncAPI: The Missing Piece in EDA

AsyncAPI solves these problems by providing:

| Feature                    | How It Helps                                                           |
| -------------------------- | ---------------------------------------------------------------------- |
| **Standard Specification** | Like OpenAPI, but for events: consistent, shareable, machine-readable. |
| **Event Documentation**    | Clarifies which channels/events exist and their structure.             |
| **Code Generation**        | Scaffolds producers/consumers in various languages.                    |
| **Visualization Tools**    | Visual editors to explore and understand complex event flows.          |
| **Validation & Testing**   | Enables spec validation, mocking, testing of async systems.            |

### Core Concepts

| Concept         | Description                                                                 |
| --------------- | --------------------------------------------------------------------------- |
| **Application** | A system that sends/receives messages over a protocol (e.g., Kafka client). |
| **Channel**     | A message path or topic, e.g., Kafka topic, MQTT topic, etc.                |
| **Message**     | Payload + metadata (headers, schema) transferred between systems.           |
| **Operation**   | `publish` (send) or `subscribe` (receive) messages on a channel.            |
| **Components**  | Reusable pieces (e.g., schemas, messages).                                  |
### Real-World Use Case

| Use Case                          | How AsyncAPI Helps                           |
| --------------------------------- | -------------------------------------------- |
| Microservices with Kafka          | Describe topics, events, message schemas.    |
| IoT with MQTT                     | Document device publish/subscribe patterns.  |
| WebSocket or Webhooks integration | Define real-time channels and message flows. |
### Sample AsyncAPI Event Flow

**Architecture:**
- **User Service** publishes `user/created`
- **Email Service** subscribes to `user/created`
- **Analytics Service** subscribes to `user/created`
    

**AsyncAPI YAML:**
```yaml
channels:
  user/created:
    publish:
      message:
        $ref: '#/components/messages/UserCreated'

components:
  messages:
    UserCreated:
      payload:
        type: object
        properties:
          id:
            type: string
          email:
            type: string
```



## Knowladge
* https://apistylebook.com/design/guidelines/
* https://twirl.github.io/The-API-Book/API.en.html