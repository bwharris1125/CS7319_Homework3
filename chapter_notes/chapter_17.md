## Chapter 17: Microservices Architecture

There is no secret group of architects who decide what the next big movement will be. Rather, it turns out that many
architects end up making common decisions.

Microservices differ in this regard - it was popularized by a famous blog entry by Martin Fowled and James Lewis.

Microservices Architecture is heavily inspired by the ideas in DDD - bounded context, decidedly inspired microservices.
Within a bounded context, the internal parts (code, data schemas) are coupled together to produce work, but they are
never coupled to anything outside the bounded context.

Each service is expected to include all necessary parts to operate independently.

Performance is often the negative side effect of the distributed nature of microservices. Network calls take much longer
than method calls. It is advised to avoid transactions across service boundaries, making determining the granularity the
key to success in this architecture.

It is hard to define the right granularity for services in microservices. If there are too many services, a lot of
communication will be required to perform work. The purpose of service boundaries is to capture a domain or workflow.

Guidelines to find the appropriate boundaries:

- purpose - a domain, one significant behaviour on behalf of the overall application
- transactions - often the entities that need to cooperate in a transaction show a good service boundary
- choreography - if excellent domain isolation require extensive communication, you may consider merging services back
  into larger service to avoid communication overhead

Microservices Architecture tries to avoid all kinds of coupling - including shared schemas and databases used as
integration points.

Once a team has built several microservices, they realize that each has common elements that benefit from similarity.
The shared sidecar can be either owned by individual teams or a shared infrastructure team. Once teams know that each
service includes a common sidecar, they can build a _service mesh_ - allowing unified control across the infrastructure
concerns like logging and monitoring.

2 styles of user interfaces:

- monolithic user interface - a single UI that calls through the API layer to satisfy user request
- micro-frontends - each service emits the UI for that service, which the frontend coordinates with the other emitted UI
  components

Microservices architectures typically utilize _protocol-aware heterogeneous interoperability_:

- protocol-aware - each service should know how to call other services
- heterogeneous - each service may be written in a different technology stack, heterogeneous means that microservices
  fully support polyglot environments
- interoperability - describes services calling one another, while architects in microservices try to discourage
  transactional calls, services commonly call other services via the network to collaborate

For asynchronous communication, architects often use events and messages (internally utilizing an event-driven
architecture).

The broker and mediator patterns manifest as choreography and orchestration:

- choreography - no central coordinator exists in this architecture
- orchestration - coordinating calls across several services

Building transactions across service boundaries violates the core decoupling principle of the microservices
architecture. DON'T.

> Don't do transactions in microservices - fix granularity instead.

Exceptions always exist (e.g. 2 different services need vastly different architecture characteristics -> different
boundaries), in such situations - patterns exist to handle transaction orchestration (with serious trade-offs).

SAGA - the mediator calls each part of the transaction, records success/failure, and coordinates results. In case of an
error, the mediator must ensure that no part of the transaction succeeds if one part fails (e.g. send a request to undo

- usually very complex). Typically, implemented by having each request in a `pending` state.

> A few transactions across services is sometimes necessary; if it is the dominant feature of the architecture, mistakes
> were made!

Performance is often an issue in microservices - many network calls, which has high performance overhead. Many patterns
exist to increase performance (data caching and replication).

However, one of the most scalable systems yet have utilized this style to great success, thanks to scalability,
elasticity and evolutionary.

Additional references on microservices:

- Building Microservices
- Microservices vs. Service-Oriented Architecture
- Microservices AntiPatterns