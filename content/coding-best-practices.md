# Software Development Best Practices

## Functional Objects Paradigm

### Functions

* Functions should be utilized extensively, either by direct invocation or through classes.
* Any logic capable of existing independently of objects should be implemented as standalone functions.
* It is recommended to maintain pure or nearly pure functions (for example following the RORO—Receive Object, Return Object—pattern).

#### Advantages

* Reduced indentation and nesting complexity.
* Simplifies function extraction from modules.
* Minimizes the likelihood of errors related to interdependencies.
* Provides a clearer and more explicit code execution flow.

#### Disadvantages

* Limited encapsulation opportunities.
* Increases the complexity of maintaining loose coupling.

### Objects

* Employ Value Objects (models) extensively, utilizing tools such as `dataclass`, `pydantic`, or ORM.
* Objects facilitate testing; thus, encapsulating the final business logic within objects is advisable.
* Classes should remain as lightweight as possible:
  * Any logic that can be externalized should be moved to standalone functions; classes should primarily serve as interfaces to these functions.
  * Avoid using `@staticmethod` and `@classmethod`.

#### Mandatory Object Usage Scenarios

* Dependency injection - need. If we need service.
* State management is required (e.g., services with injected sessions).
* A clear hierarchy is necessary for code reuse through inheritance (although this scenario is relatively rare).
* Extensive variable passing between functions is required, effectively constituting a state.

#### Naming Conventions for Business Logic Objects

* `XxxConnector`
* `XxxManager`
* `XxxService`
* `XxxRepository`
* `Core`

## File Naming suggestions for LLM apps

### Communication

* View modules responsible for transforming domain data into formats suitable for LLM.
* Domain-specific data transformation nuances should reside within their respective domains.

### Formatting

* Modules handling formatting tasks, particularly prompt formatting.
* Function format:

```python
def fmt_<name>_prompt(*args: tuple[Any], **kwargs: dict[str, Any]) -> dict[str, str]:
```

* Function names should be verbose (`fmt_<name>_prompt`).
* Return type: dictionary (`dict[str, str]`), with the first argument indicating the message type (`user`, `system`).

### Other Directories

* `core`
* `model(s)`
* `schema(s)`

## Additional info
https://github.com/zhanymkanov/fastapi-best-practices?tab=readme-ov-file#cpu-intensive-tasks