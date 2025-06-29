## Layered architecture
https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html

The **layered architecture pattern**—also known as **n-tier architecture**—is one of the most widely used architectural styles in enterprise software development, especially in Java EE applications. It organizes software into **horizontal layers**, each with a specific role such as presentation, business logic, persistence, and data access. This separation helps maintain **modularity** and **clear responsibilities** within the system.
### Key Concepts and Benefits
Each layer acts as an **abstraction**, communicating only with the layer directly below or above it. This concept of **"closed layers"** enforces a clean structure and promotes **layer isolation**, making the system easier to test, maintain, and evolve. It also ensures that changes in one layer typically don’t impact others.
Sometimes, **"open layers"** are introduced to improve flexibility. For example, a shared services layer might be bypassed by business logic when direct access to data is needed, helping to avoid unnecessary complexity or bottlenecks.
### Practical Example
In a typical use case—retrieving customer information—the request flows from a user interface through business and persistence layers to the database, and back. Each component only handles the logic relevant to its layer, improving testability and ease of development.
### Considerations and Drawbacks
While layered architecture is intuitive and well-suited for teams organized by functional roles, it has notable limitations:
- **Low agility and deployment speed**, often resulting in monolithic systems.
- **Performance and scalability issues** due to multiple layer hops for simple requests.
- **Architecture sinkhole anti-pattern**, where layers pass data without meaningful processing.
### Pattern Ratings
- **Ease of Development**: High
- **Testability**: High
- **Agility, Deployment, Performance, Scalability**: Low
### **Common Layers in a Layered Architecture**
1. **Presentation Layer**
    - Manages the user interface and browser-based interaction.
    - Handles input, output, and formatting for the user.
2. **Business Layer**
    - Contains the core business logic, rules, and domain processes.
    - Coordinates data flow between the presentation and persistence layers.
3. **Persistence Layer**
    - Manages access to data sources like databases or APIs.
    - Implements data access logic through DAOs or repositories.
4. **Database Layer**
    - Represents the actual database or data store.
    - Stores structured information used by the application.
5. **Service Layer (optional)**
    - Provides reusable services such as logging, caching, and auditing.
    - Often shared across multiple business modules.
6. **Integration Layer (optional)**
    - Handles communication with external systems or third-party APIs.
    - Acts as a bridge between internal business logic and external services.
7. **Security Layer (optional)**
    - Manages authentication, authorization, and other security concerns.
    - Can span multiple layers depending on implementation.
## DDD
* https://hackernoon.com/my-ddd-cheat-sheet-ue2n30g5
* https://gist.github.com/percyvega/aae9b3744378e619f1ae73b48e172b3a
* https://medium.com/yanchware/domain-driven-design-a-cheat-sheet-722a4e4f9c7f
**Domain-Driven Design (DDD)** is an approach to software development that focuses on deeply understanding the domain—the subject area the software is addressing. Coined by Eric Evans in his 2004 book, _"Domain-Driven Design: Tackling Complexity in the Heart of Software,"_ DDD prioritizes modeling software to align closely with real-world business concepts.
### Key Concepts of DDD:

#### 1. **Domain**
The domain is the sphere of knowledge, business activity, or the problem the application is trying to solve. Understanding the domain thoroughly allows developers to create more relevant and effective software.
#### 2. **Ubiquitous Language**
DDD emphasizes establishing a common, shared language among all stakeholders—including developers, business analysts, and domain experts. This language becomes the backbone of communication, ensuring clarity, consistency, and alignment between software models and the business domain.
#### 3. **Bounded Context**
A bounded context defines clear boundaries around a particular area of the domain, establishing a distinct linguistic and conceptual space. Each bounded context maintains its own domain models, terminology, and rules, reducing complexity and confusion.
#### 4. **Entities**
Entities are domain objects uniquely identifiable by an identity that persists over time, independent of attribute changes. For instance, a customer entity retains its identity even if its address or name changes.
#### 5. **Value Objects**
Value objects represent immutable objects whose identity is defined entirely by their attributes. They’re typically small and easily replaceable. Examples include an address or a monetary value.
#### 6. **Aggregates**
Aggregates cluster related objects (entities and value objects) into cohesive units, managed as a single transactional consistency boundary. Each aggregate has a root entity that controls access to the aggregate.
#### 7. **Repositories**
Repositories abstract data storage details, providing collection-like interfaces for accessing and managing aggregates. They help keep the domain model independent from specific storage mechanisms.
#### 8. **Domain Services**
Domain services encapsulate domain logic that doesn't naturally fit into entities or value objects, often representing actions or calculations involving multiple objects.
#### 9. **Context Mapping**
Context mapping visualizes relationships and integrations between different bounded contexts. It helps teams manage interactions and integrations effectively, ensuring consistency and clearly defining communication channels.
## MVC
**Model-View-Controller (MVC)** is a widely-used architectural pattern that separates an application into three interconnected components: the **Model**, **View**, and **Controller**. The primary goal of MVC is to organize code logically, making applications easier to manage, scale, and maintain.

### Key Concepts of MVC:

#### 1. **Model**
The Model manages data, logic, and rules of the application. It represents the core business logic, including data retrieval, manipulation, validation, and state management. The Model is independent of the user interface.
- **Examples:** Databases, business logic, domain objects.
### 2. **View**
The View handles the display of data (the user interface). It presents information to the user and sends user actions (e.g., button clicks, form submissions) to the controller. Views typically have minimal logic—focusing on presentation rather than computation or data manipulation.
- **Examples:** HTML templates, UI components, visual layouts.
### 3. **Controller**
The Controller serves as an intermediary, managing the interaction between the Model and the View. It receives input from users via the View, processes requests, interacts with the Model to retrieve or modify data, and updates the View accordingly.
- **Examples:** Handling HTTP requests, user interactions, events processing.
### MVC Workflow:
- A user interacts with the **View** (e.g., submitting a form).
- The **Controller** receives and processes the user input.
- The **Controller** interacts with the **Model**, retrieving or updating data.
- The updated data from the **Model** is returned to the Controller, which updates the **View** to reflect these changes.
### Advantages of MVC:
- **Separation of Concerns:** Clearly separates responsibilities, enabling easier testing and maintenance.
- **Scalability:** Facilitates the addition of new features without disrupting existing ones.
- **Flexibility:** Allows independent modifications or replacements of the Model, View, or Controller.

## ReMVP
_Recursive Model View Presenter_
### Introduction

**ReMVP (Recursive Model-View-Presenter)** is an architectural pattern designed specifically for modular, composable, AI-driven applications. It adapts the principles of **MVP (Model-View-Presenter)**, on AI system by adding recursion (because we can have app sized subagents) emphasizing clear boundaries, scalability, and modularity at multiple abstraction levels.

> Note: The use of DDD and additional elements and layered architecture is strongly recommended.
### Core Principles

#### 1. Recursive Structure
Every component (function/module) can itself be structured as a smaller MVP-based module, allowing modular composition of complex workflows from simpler building blocks.

#### 2. Clear Separation of Concerns
Each module strictly divides responsibilities:
- **Model (Domain Layer):** Business logic, rules, and entities.
- **Presenter:** Data mapping, formatting, translating Model data to a presentation-ready state.
- **View:** Interfaces for user interaction, APIs, or external system integration.
#### 3. Integration with Domain-Driven Design

Use DDD to structure the Model layer clearly:
- **Entities & Value Objects:** Define clear domain models.
- **Use Cases:** Represent business operations.
- **Repositories & Connectors:** Abstract external system interactions and persistence.
### Component Layers

Each module/component follows:
#### View
- Handles interaction with the user or external systems.
- Responsible only for rendering data or accepting input.
#### Presenter
- Acts as the intermediary between View and Model.
- Transforms and maps data (DTO ↔ Domain entities).
- Handles presentation logic (formatting, filtering, adapting).
- Maintains UI-specific state but contains no business logic.
#### Model (DDD-inspired)
- **Entities/Value Objects:** Core domain data structures.
- **Use Cases:** High-level business logic and workflows.
- **Domain Services:** Common logic shared across use cases.
- **Repositories & Connectors:** External integration abstractions (APIs, databases).
#### Orchestrator (Optional but Common)
- Manages complex interactions between multiple MVP modules.
- Coordinates asynchronous and synchronous workflows.
#### Recursive Composition
- Modules are composable, allowing a function (like "Summarize") to be structured as a smaller MVP module:
    - Outer MVP → Inner MVPs (smaller functions/workflows).

### Example Project Structure

```
my_app/
├── main.py
├── modules/
│   └── summarization_module/
│       ├── view/          # UI, API endpoints
│       ├── presenter/     # Data mappers, formatters
│       ├── model/         # Domain logic, use cases, entities
│       └── orchestrator/  # Workflow control
├── infrastructure/        # Persistence, connectors
├── shared/                # Shared utils, entities, interfaces
└── config/
```

### Best Practices
- **Thin Presenter:** Only mapping and formatting; no business logic.
- **Clean Boundaries:** Use DDD principles to isolate core business rules.
- **Interface-Oriented Design:** Modules communicate through well-defined interfaces.
- **Recursive Clarity:** Clearly document nested MVP layers and data flows to avoid confusion.
- **Testing Strategy:** Each MVP module is independently testable, mockable, and replaceable.
    
### Example Workflow

```
User Request
   ↓
View (UI/API/communication for Funcion)
   ↓
Presenter (DTO ↔ Domain)
   ↓
Model (UseCase, Entities, Repositories)
   ↓
Orchestrator (if complex workflows)
   └── MVP sub-module (recursive structure repeats)
```



