## JSON API
https://jsonapi.org/
A formal standard for serialising data into JSON for REST-like services. Describes resource structure, pagination, sorting, links and errors to produce uniform APIs.

In JSON:API, every resource is uniquely identified by a combination of a `type` and an `id`. The `type` defines the category of the resource (e.g., "articles", "users"), while the `id` identifies a specific instance within that type. This pairing ensures global uniqueness and consistent resource identification across the API. It is especially important for maintaining uniformity in API responses, supporting side-loaded (included) related data, and handling collections that may contain mixed resource types. By explicitly including both `type` and `id`, JSON:API allows clients to interpret and cache resources correctly, even when the context of the original request URL is unavailable.
### Main Concepts of JSON API
1. **Media Type**
	Content-Type: `application/vnd.api+json`
2. **Resources**
Everything is treated as a **resource** with a `type` and `id`.
```json
{
  "data": {
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON API is awesome"
    }
  }
}
```
3. **Attributes**
Non-relational data goes under the `attributes` key.
```json
"attributes": {
  "title": "Hello",
  "content": "World"
}
```
4. **Relationships**
Related resources are declared in the `relationships` object.
```json
"relationships": {
  "author": {
    "data": { "type": "people", "id": "9" }
  }
}
```
5. **Included (Compound Documents)**
Related resources can be side-loaded via `included`.

```json
"included": [
  {
    "type": "people",
    "id": "9",
    "attributes": {
      "name": "Jane"
    }
  }
]
```
6. **Fetching Resources**
Single: `GET /articles/1`
Collection: `GET /articles`
7. **Filtering**
```http
GET /articles?filter[author]=12
```
8. **Sorting**
```http
GET /articles?sort=-created,title
```
9. **Pagination**
```http
GET /articles?page[number]=2&page[size]=10
```
10. **Creating a Resource**
```http
POST /articles
Content-Type: application/vnd.api+json

{
  "data": {
    "type": "articles",
    "attributes": {
      "title": "New Post",
      "content": "..."
    }
  }
}
```
11. **Updating a Resource**
```http
PATCH /articles/1

{
  "data": {
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Updated Title"
    }
  }
}
```
12. **Deleting a Resource**
```http
DELETE /articles/1
```
13. **Errors**
Standardized error responses:
```json
{
  "errors": [
    {
      "status": "400",
      "title": "Invalid Attribute",
      "detail": "Title cannot be blank"
    }
  ]
}
```
### Summary

| Feature        | JSON API Practice          |
| -------------- | -------------------------- |
| Resource ID    | `"type"` + `"id"`          |
| Content-Type   | `application/vnd.api+json` |
| Relationships  | Under `relationships` key  |
| Linked data    | Use `included`             |
| Filters, Sorts | Use URL params             |
| Stateless      | Yes                        |
## JSON-RPC
https://www.jsonrpc.org/specification

**JSON-RPC** is a **remote procedure call (RPC)** protocol encoded in **JSON**.  
It's:
- **Lightweight**
- **Transport agnostic** (can use HTTP, WebSockets, etc.)
- **Request–response** based
- Supports **notifications** (calls without a response)
### Core Concepts

**Request Object:**

```json
{
  "jsonrpc": "2.0",
  "method": "subtract",
  "params": [42, 23], // or {"minuend": 42, "subtrahend": 23}
  "id": 1
}
```

- `jsonrpc`: must be `"2.0"`
- `method`: method name to invoke
- `params`: (optional) method parameters (positional or named)
- `id`: unique identifier for matching response

**Response Object:**

```json
{
  "jsonrpc": "2.0",
  "result": 19,
  "id": 1
}
```

- `result`: the result of the call (if successful)
- `error`: object if an error occurred
- `id`: must match the request `id`

**Error Object:**

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32601,
    "message": "Method not found"
  },
  "id": 1
}
```

- `code`: standard or custom error code
- `message`: error message
- `data`: (optional) additional error data
    

**Standard Error Codes:**

| Code    | Meaning               |
| ------- | --------------------- |
| -32700  | Parse error           |
| -32600  | Invalid Request       |
| -32601  | Method not found      |
| -32602  | Invalid params        |
| -32603  | Internal error        |
| -32000+ | Server error (custom) |

**Notification (No Response Expected):**

```json
{
  "jsonrpc": "2.0",
  "method": "update",
  "params": [1, 2, 3, 4]
}
```

- No `id` field → server doesn't reply

**Batch Requests:**

```json
[
  {
    "jsonrpc": "2.0",
    "method": "sum",
    "params": [1,2,4],
    "id": 1
  },
  {
    "jsonrpc": "2.0",
    "method": "notify_hello",
    "params": [7]
  }
]
```

### Summary Table

| Feature       | Supported | Notes                       |
| ------------- | --------- | --------------------------- |
| Method calls  | ✅         | With `id`                   |
| Notifications | ✅         | No `id`, no response        |
| Batching      | ✅         | Array of requests           |
| Errors        | ✅         | Standard & custom supported |
| Transport     | 🚫         | Not specified by protocol   |
## JSend
https://github.com/omniti-labs/jsend

**JSend** is a **standard specification** for formatting **JSON responses** in web applications (especially REST APIs).  
It ensures **consistent** structure for success/failure/error messages in application-level responses.

- Encourages **uniform API responses**
- Helps **frontend/backend teams** understand responses easily
- Simplifies debugging and **reduces custom error handling**
- Offers a lightweight layer atop HTTP status codes

### Response Types & Structure

JSend defines **three types of responses**:

| Type    | Description                                              | Required Keys       | Optional Keys  |
| ------- | -------------------------------------------------------- | ------------------- | -------------- |
| success | Request processed correctly and returned data (or null). | `status`, `data`    |                |
| fail    | Invalid input, validation error, or unmet condition.     | `status`, `data`    |                |
| error   | Server-side or unexpected failure.                       | `status`, `message` | `code`, `data` |
### Success Example

Typical API call succeeded:

```json
{
  "status": "success",
  "data": {
    "post": { "id": 1, "title": "A blog post", "body": "Some useful content" }
  }
}
```

### Empty data (e.g. delete):

```json
{
  "status": "success",
  "data": null
}
```
### Fail Example

Validation or input error:

```json
{
  "status": "fail",
  "data": {
    "title": "A title is required"
  }
}
```

> Use `fail` when the problem is **user-submitted** data (not server failure).

### Error Example

Server error or exception:

```json
{
  "status": "error",
  "message": "Unable to communicate with database"
}
```

With optional error code and extra data:

```json
{
  "status": "error",
  "message": "Database timeout",
  "code": 504,
  "data": {
    "timeout": "30s",
    "retry": true
  }
}
```

---

### HTTP Compatibility

- **Still use HTTP status codes** appropriately (e.g. 200, 400, 500).
- JSend provides a **uniform payload format**, useful especially when the HTTP response is **not directly visible to client-side logic** (e.g., in JSONP, embedded script blocks, etc.).

### Summary of Keys

| Key       | Type        | Notes                                                   |
| --------- | ----------- | ------------------------------------------------------- |
| `status`  | string      | `"success"`, `"fail"`, or `"error"`                     |
| `data`    | object/null | Required for `success` and `fail`; optional for `error` |
| `message` | string      | Required only for `error`                               |
| `code`    | number      | Optional in `error`                                     |
### When to Use Each Type
- Use **`success`** when things work as expected.
- Use **`fail`** for client-side mistakes or validation issues.
- Use **`error`** when the server failed (crashed, exceptions, etc.).

## Problem details
RFC 7807 problem details
+ https://swagger.io/blog/problem-details-rfc9457-doing-api-errors-well/
+ https://www.codecentric.de/en/knowledge-hub/blog/charge-your-apis-volume-19-understanding-problem-details-for-http-apis-a-deep-dive-into-rfc-7807-and-rfc-9457
+ https://datatracker.ietf.org/doc/html/rfc7807

**RFC 7807** defines a **standard format** for expressing errors in HTTP APIs using a JSON (or XML) object called the **Problem Details Object**.  
**RFC 9457** extends this by improving flexibility, interoperability, and support for modern use cases.

**Future Outlook:**
- Better i18n/l10n support
- Smarter debugging tools
- AI-enhanced error classification
- Greater community contribution to problem type registries
### Basic Structure of a Problem Details Object (RFC 7807)

```json
{
  "type": "https://example.com/probs/out-of-credit",
  "title": "You do not have enough credit.",
  "status": 403,
  "detail": "Your current balance is 30, but that costs 50.",
  "instance": "/account/12345/msgs/abc"
}
```

| Field      | Required | Description                                                          |
| ---------- | -------- | -------------------------------------------------------------------- |
| `type`     | No       | A URI identifying the problem type. Can link to human-readable docs. |
| `title`    | No       | Short, human-readable summary of the problem type.                   |
| `status`   | No       | HTTP status code for this occurrence.                                |
| `detail`   | No       | Human-readable explanation of the specific problem.                  |
| `instance` | No       | URI that identifies this specific problem occurrence.                |

### Additions with RFC 9457
- **Timestamp** (e.g., `timestamp: "2023-11-28T12:34:56Z"`): When the error occurred.
- **Custom Fields**: Arbitrary additional members allowed, e.g., `"custom-field": "context info"` — enables extensibility.
- **Better support** for:
	- Interoperability across APIs
	- Complex/multi-faceted error reporting
	- Security-conscious messaging

### Benefits
- **Consistency**: No need for custom error formats.
- **Machine-readability**: Easily parsable by clients.
- **Extensibility**: Can be expanded with custom fields.
- **Transparency**: More helpful error messages without leaking sensitive info.

### Example in Practice

**Request**:

```
GET /api/books/12345 HTTP/1.1
```

**Response**:

```json
{
  "type": "https://bookstore.example.com/problems/book-not-found",
  "title": "Book Not Found",
  "status": 404,
  "detail": "The book with ID 12345 could not be found in our database.",
  "instance": "/api/books/12345",
  "timestamp": "2023-11-28T12:34:56Z",
  "custom-field": "Additional context here"
}
```

### Best Practices
- Define your own `type` URIs for custom errors.
- Ensure all error responses follow this format (content type: `application/problem+json`).
- Do not leak internal info in `detail`.
- Document all custom fields and types.
- Keep it human-readable yet machine-parseable.

### Common Use Cases

- 404 Not Found
- 403 Forbidden (e.g., quota exceeded, payment required)
- 422 Validation Failed
- 500 Internal Error (use with care)

### Tools/Libraries
Many frameworks support RFC 7807 out of the box or with plugins:
- Spring Boot (`ProblemDetail`)
- .NET (`ProblemDetails`)
- Flask/Django with custom middleware
- Express.js with middleware libraries

## ACP
_Agents Context Protocol_

**ACP** defines a structured standard for agent-to-agent and agent-to-app communication in agentic systems. It is designed to formalize how agents describe interactions, encapsulate responses, and handle both synchronous and asynchronous workflows.

>  **Note**: ACP is not a transport protocol. It standardizes payload structures and response semantics, and can be transported over REST, AsyncAPI, or other stateless formats.
### Key Principles
- **Modular & Embeddable**: ACP can be implemented as a module function for LLM within your system codebase, not necessarily a standalone service.
- **Standard Documentation**: ACP-based services **must be documented** using TypeSpec, OpenAPI, or AsyncAPI with description that will be given to LLM.
- **Versioned APIs**: Adopt REST-style versioning (e.g., `/v1/search_web/`).
- **Transport Flexibility**: REST-style is suitable and preferred for basic requests. But we can use RPC (e.g., gRPC or WebSockets) for long-running tasks with statuses or intermediate data.
- **Streaming and Messages Friendly**: Even streaming and messages responses **must follow ACP** structure.
- **Stateless Preferred**: Agent interfaces should be designed stateless unless unavoidable.
- **Security**: For authorisation, use the methods available in the selected transport type. (eg. **JWT** or **Basic Auth**).
### Design Influences
ACP is inspired by:
- [JSend](https://github.com/omniti-labs/jsend)
- RFC 7807 Problem Details
- **MCP**  modular context protocol.

> **N️ote:** Use REST, AsyncAPI, or other stateless protocols. Prefer **not** to use MCP to avoid redundant overlap.
### Request Structure

* There must be at least one of fields:
	* `query`
	* `dialogue`
	* `params`

| Field           | Type   | Description                                    |
| --------------- | ------ | ---------------------------------------------- |
| `query`         | string | User input or prompt.                          |
| `dialogue`      | array  | OpenAI-style message history.                  |
| `caller`        | object | Metadata about the calling agent.              |
| └ `name`        | string | Name or ID of the caller.                      |
| └ `role`        | string | Role (e.g., planner, worker).                  |
| └ `trust_level` | string | Security context (e.g., `high`, `low`).        |
| `params`        | object | Additional task parameters (custom).           |
| **Header**      | string | `X-Request-Id: <uuid>` – Required for tracing. |
### Responses
#### Response Types

ACP supports 6 standardized response `result` types:

| Result Type  | Purpose                                                                                         |
| ------------ | ----------------------------------------------------------------------------------------------- |
| `success`    | The request was processed successfully.                                                         |
| `in_process` | Used during streaming or long-running tasks.                                                    |
| `answer`     | Used when a sub-agent responds directly to a user.                                              |
| `signal`     | For events (for example: more information or human help request). Type must be send by `state`. |
| `error`      | Server-side or unexpected failure.                                                              |
| `fail`       | The input was invalid or failed validation.                                                     |
#### Success / In-Process / Answer Responses
These response types can include the following:
##### Common Fields
* Response must contain at least one of:
	* `text` - for llm data.
	* `data` - for system data.
	* `state` - to send status.

| Field    | Type   | Description                                                                                    |
| -------- | ------ | ---------------------------------------------------------------------------------------------- |
| `result` | string | One of: `success`, `in_process`, or `answer`. For status type to understand response or chunk. |
| `text`   | object | LLM-related context.                                                                           |
| `data`   | object | Non-text data (e.g., UI components, params, structured responses).                             |
| `state`  | object | Optional; progress/state tracking for long tasks.                                              |
##### `text` Object
* Object must contain at least one of:
	* `content` - for main content.
	* `rules` - for content rules.
	* `examples` - for examples about how to use content.

| Field      | Type   | Description                                   |
| ---------- | ------ | --------------------------------------------- |
| `content`  | string | Main input for LLM.                           |
| `rules`    | string | Task rules or constraints.                    |
| `examples` | string | Illustrative examples.                        |
| `language` | string | Language code (e.g., `en`). Required default. |
| `format`   | string | Format (e.g., `md`, `raw`). Required default. |
##### `data` Object

- Holds structured data (e.g., buttons, options).
- Should follow best practices of REST object design.
- May include a `schema` field (URL or JSON Schema) for validation.
##### `state` Object (Optional)

| Field      | Type   | Description                                         |
| ---------- | ------ | --------------------------------------------------- |
| `type`     | string | Enum status type (e.g., `searching`, `need_human`). |
| `title`    | string | Short, human-readable summary.                      |
| `detail`   | string | Detailed explanation.                               |
| `progress` | number | Percent completion (0–100).                         |
> Tip: Also there can be both `text` and `data` field, but they do not conflict (we just print ui for example and answer with llm based on text).
#### Schema
```schema
{
  result: "success" | "in_process" | "answer";
  text?: {
    content?: string;
    rules?: string;
    examples?: string;
    language?: string;  // default: "en"
    format?: "raw" | "md";  // default: "raw"
  };
  data?: {
    schema?: string;  // URI
    [key: string]: any;
  };
  state?: {
    type: string;
    title: string;
    detail?: string;
    progress?: number;  // 0–100
  };
}
```

#### Error/Fail responses

Two distinct failure types:

| Type    | Use Case                                 |
| ------- | ---------------------------------------- |
| `fail`  | User or input error (e.g., bad query).   |
| `error` | Internal/server failure (e.g., timeout). |
##### Error/Fail Fields

| Field        | Type   | Required | Description                                            |
| ------------ | ------ | -------- | ------------------------------------------------------ |
| `result`     | string | +        | Either `fail` or `error`.                              |
| `type`       | string | -        | URI to documentation of error type.                    |
| `title`      | string | -        | Short, human-readable summary.                         |
| `detail`     | string | -        | Detailed description of the problem.                   |
| `instance`   | string | -        | URI for specific error occurrence.                     |
| `code`       | string | -        | Application-specific error code.                       |
| `data`       | object | -        | Optional UI hints (e.g., retry timeouts, suggestions). |
| `timestamp`  | string | -        | ISO 8601 timestamp.                                    |
| `request_id` | string | +        | Required in `X-Request-Id` header.                     |
### Streaming & message based API.

- Streaming responses should use `result: in_process`.
- Each chunk should include a partial `text` or `rules` field.
- Chunks MUST conform to the standard schema.
### Examples

> In each there is `request_id` in `X-Request-Id` header.

**Basic (we call add(2, 2)):**
```json
{
  "result": "success",
	"text": {
		"content": "2 + 2 = 4",
		"language": "any", // client can throw error
		"format": "raw" // client can throw error
	}
}
```
**Status (when we used RPC and called during the task):**

```json
{
  "result": "in_process",
	"state": {// optional
		"title": "Searching the web!",
	}
}
```
**Streaming chunk with content:**

```json
{
  "result": "in_process",
	"text": {
		"content": "you", // token in chunk
		"language": "eng",
		"format": "md"
	}
}
```
**Streaming chunk with rules:**
```json
{
  "result": "in_process",
	"text": {
		"rules": "must", // token in chunk
		"language": "eng",
		"format": "md"
	}
}
```
**Data only:**
```json
{
  "result": "success",
	"data": {
	    "post": { "id": 1, "title": "A blog post", "body": "Some useful content" }
	}
}
```