# Top Cloud Messaging Design Patterns

Jose Antonio Muro - Centro de Novas Tecnoloxías de Galicia

------

**Available resources**

- [Xornadas CNTG](https://cntg.xunta.gal/web/cntg/xornadas/-/detalle/693202)

🏷️ Tags: `talk`, `2024`, `cloud`, `architecture`, `infrastructure`, `patterns`

------

## General notes (mostly Spanish)

### Falacies dentro de la computación distribuida

* La red es fiable. Falacia, no podemos asumir que la red es fiable y/o está disponible
  * timeout, circuit breaker, quque-base design patterms, ...
  * Para mitigar este problema:
    * Patrones de retry, pero con ojo
    * Protegernos de sistemas externos
    * Patrón circuit breaker: si se alcanza cierto número de retries, se abre el circuito (se sale del mecanismo de retry)
* La latencia es cero. En SD la latencia nunca es nula
  * Atender a la latencia cuando se producen distintas llamadas concatenadas a microservicios
  * Evitar llamadas innecesarias -> cachear / usar cache
  * Para mitigar este problema:
    * Caching strategies like cache-aside, bulk requests static content hosting near clients
* Ancho de banda infinito. Nunca es infinita, ni la capacidad de computación lo es tampoco
  * Para mitigar este problema:
    * Throttling policies
    * Small payloads such as claim-check patterns
      * Mensajes con menos información, con URI a recuperar información más pesada: reduce tamaño de mensajes, optimiza recursos, recuperación de información pesada solo cuando se necesite (uso explícito de la URI)
* La topología no cambia. Nada está exento a cambio
  * Para mitigar este problema:
    * Herramientas de Service discovery
    * No hardcoding IPs
    * Offload cross-curring concerns
* Asumir que siempre hay un SysOp que se encargue de operaciones. No es siempre así
  * Para mitigar este problema:
    * Embrace DevOps
    * Reduce Bus Factor dependency
      * Esto es la probabilidad de que a nuestro SysOp le pase algo (eg, tenga un accidente)
      * No depender de personas específicas. Que se reparta el conocimiento
* El coste de transporte es cero
  * Para mitigar este problema:
    * Cost calculation
    * Choose best protocol for your use case suck as JSON, gRPC, etc.
* Component versioning is simple. Luego se demustra que no fue tan simple ...
  * Para mitigar este problema:
    * Embrace continuous optimization culture
    * Know your platform
    * Focus on key components and store insightful logs
    * Automatize alerts on critical events
    * Create standard data logging formats
    * Ensure data can be aggregated and centralized

### Cloud pilars and design patterns 

#### Reliability

The ability of a system to recover from failures and continue to work properly.

Patrones de diseño:

* **Design for self-healing**
  * Apply retry, circuit breaker, bulkhead, load-leveling, throttling patterns, etc.
* **Apply redundancy**
  * Use replication, health probes, automatic service discovery mechanisms for dynamic connectivity, automatic failovers, etc.

#### Operational Excellence

Operations processes that keep a system running in production without issues or outages.

(en resumen, aplicar Cultura DevOps)

Patrones de diseño:

* **Establish development standards (DevOps)**
  * Apply solid principles, quality code policies, clean architectures, offload cross-cutting concerns, use event-driven architectures, etc.
* **Design for operations (DevOps)**
  * Automate for efficiency (CI/CD, laC), adopt safe deployment practices, evolve operations with observability, testing, monitoring and versioning.

#### Performance Efficiency

The ability of a system to adapt to changes in load with no loss of performance.

* **Negotiate realistic performance targets**
  * Make an effort to fully understand your use case and collaborate with business owners.
* **Design to meet elastic capacity requirements**
  * Scale out as load increases and scale in when the extra capacity is not needed.
*  **Use best data store as per your scenario**
  * Embrace polyglot persistence scenarios, specially in distributed or microservices-oriented applications.
* **Continuous optimization**
  * Build a culture of performance-driven optimization, proactively monitor performance patterns and fine-tune apps.

Patrones de diseño:

* Asynchronous request-reply
* Cache-aside
* Bulkhead
* Claim check
* CQRS
* Index table
* Materialized view
* Sharding
* Sidecar
* Static content hosting
* etc.

#### Cost Optimization

Managing costs to maximize the value delivered aligned with business needs and budgets.

Patrones de diseño:

* **Optimize for business needs and costs**
  * Align design with business and cost needs considering SLAs, RTOS, RPOs, etc.
* **Design to scale out / scale in**
  * Scale out as load increases and scale in when the extra capacity is not needed.

Patrones de diseño:

* Claim check
* Competing consumers
* Publisher/subscriber
* Queue-based load leveling
* Throttling
* etc.

#### Security

Protecting applications and data from threats.

### Architecture styles

#### N-Tiers

* **N-Tier architecture**. Divides an application into logical layers and physical tiers

  * **Layers** are a logical way of reflecting internal architecture, separating responsibilities and manage dependencies between software components.
  * **Tiers** are a physical way of separating layers and usually run on separate machines,

Cuándo usarla

* Simple web applications or traditional on-premises applications
* There is a need for simple portability between cloud and on-premises, and between cloud platforms with minimal refactoring

#### Web QUeue Worker

* A Web Queue Worker architecture typically is made of:
  * A Web App or Web APl that serves client requests
  * A Worker that performs resource-intensive tasks, long-running workflows, or batch jobs
  * A Queue to provide asynchronous communication between web front end and worker.
* The front end might consist of a web API. On the client side, the web API can be consumed by a single-page application that makes AJAX calls, or by a native client application.

Cuándo usarla

* Applications with simple domain
  Applications with some long-running workflows or batch operations
* When you want to use managed services, rather than infrastructure as a service (laaS)

#### Event Driven

* An event-driven architecture consists of event producers that generate a stream of events, and event consumers that listen for the events.
* There are different event-driven flavours such as:
  * **Pub/Sub**: When an event is published, it sends the event to each subscriber. After an event is received, it can't be replayed, and new subscribers don't see the event.
  * **Event Streaming**: Events are written to a log. Events are strictly ordered (within a partition) and durable. Clients don't subscribe to the stream, instead a client can read from any part of the stream. The client is responsible for advancing its position in the stream. That means a client can join at any time, and can replay events.

Cuándo usarla?

* Multiple subsystems must process the same events.
* Real-time processing with minimum time lag.
* High volume and high velocity of data, such as loT
* There is a need to decouple consumers and producers yet providing asynchronous
integration with eventual consistency

#### Microservices

* A Microservices architecture consists of a collection of small, independent, autonomous, self-contained and loosely coupled services.
* Each service is self-contained and should implement a single business capability within a bounded context, improving scalability and fault isolation
* Services can be developed and deployed independently improving agility. A team can update an existing service without redeploying the entire application.
* Each service is a separate codebase, which can be managed by a small development team, improving productivity while lowering management overhead caused in large teams.
* Services are responsible for persisting their own data or external state.
* Services communicate with each other by using well-defined and versioned contracts and APls.
* Supports polyglot programming, data persistence and mix of technologies

Retos de esta arquitectura:

* **Complexity**. Each service is simple but the entire system is much more complex.
* **Development and testing** in a distributed environment. Refactoring across service boundaries can be very hard and test service dependencies is a big challenge in very fast evolving applications.
* **Network congestion and latency**. Avoid chatty synchronous communications, think of serialization formats and embrace asynchronous communication patterns.
* **Data Integrity**. Data consistency is a challenge. Embrace eventual consistency whenever possible. Apply saga or compensating transaction patterns to deal with transactional failures.
* **Management**. A mature DevOps culture is a must. No sense it only a small low-skilled team must manage all microservices.
* **Versioning**. Updates to a service must not break services that depend on it.
0 Skill Set. Lack of gobernance. You might end up with so many different languages and frameworks that the application becomes hard to maintain


#### Uber Microservices Orientes Architecture Summary

Caso de Uber. Problemas que tenían con su arquitectura en monolito

* Extremely complex which made develop new features and fix bug very
hard
* Obstacle for continuos deployment
* Unable to scale efficiently and separately as per funcionality requirements. Only scale by "cloning all application".
* Reliability was a problem because all modules run in the same process
* Extremely difficult to adopt new frameworks and languages

Cómo lo solucionan / atacan:

* Uber solved issues by adopting Microservice architecture:
* Each microservice is a mini-application that has its own hexagonal architecture consisting of business logic along with various adapters
* Each microservice is simpler than the previous whole monolith.
* Microservices can be scaled independently aligned with peak loads based on most used app functionality. Support scale by "splitting different things or functionality". See scale cube for details.
* Microservices can be deployed separately.


#### Modular Monolyth

* "Choose microservices for the benefits, not because your monolithic codebase is a mess" — Simon Brown

Características

* The biggest difference between Modular monoliths and Microservices is how they are deployed.
* Microservices elevate the logical boundaries inside a modular monolith into physical boundaries.
* The problem is **people end up using microservices to enforce code boundaries**. * **Modular monoliths** give you high cohesion, low coupling, data encapsulation, focus on business functionalities, and more.
* **Microservices** give you all that, plus independent deployments and independent scalability plus the ability to use different technology stacks per service.

### Cloud Archiecture Bootcamp

#### Asynchrongus vs Synchronous Worlds

#### Synchronous

* The client expects a timely final response from the target service and might even block while it waits
* Request / Response (one-to-one)
  * A client makes a request to a service and waits for a response.
  * The client expects the final response to arrive in a timely fashion.
  * In a thread-based application, at single process scope, the thread that makes the request might even block while waiting. Use always async / await in Input/Output (10) operations to not block threads.
  * Main drawback of this type of integration is that even using async / await 10 calls other resources different from threads are consumed (for instance, sockets) so that some issues could arise such as loss of requests or exhaustion of client resources.

##### Asynchronous

* Async Request / Response (one-to-one)
  * A client sends a request to a service, which replies asynchronously. The client does not block while waiting and is designed with the assumption that the response might not arrive for a while.
* Async Publish / Responses (one to many)
  * A client publishes a request message. It waits a certain amount of time for responses from interested services.
* Notification (one-to-one)
  * A client sends a request to a service but no reply with content payload is expected or sent.
* Publish / Subscribe (one to many)
  * A client publishes a notification message, which is consumed by zero or more interested services. No response with content payload is expected from subscribers

Patrones de diseño

* Asynchronous Request-Response
  * The client sends a request and receives an HTTP 202 (Accepted) response.
  * The client sends an HTTP GET request to the status endpoint. The work is still pending, so this call returns НТТР 200.
  * At some point, the work is complete and the status endpoint returns 302 (Found) redirecting to the resource.
  * The client fetches the resource at the specified URL.

* Quque-bases Asynchronous Messaging patterns
  * QUé permite este patrón?
    * Decoupling workloads
    * Cross-Platform Integration
    * Reliable Asynchronous workflows
    * Temporal Decoupling
    * Queue-Based Load Leveling
  * En qué se basa?
    * Senders can POST or PUSH a message to the queue.
    * Receivers can RETRIEVE or PULL message from the queue
      * Normally the message is removed if retrieved successfully. Otherwise, the message remains in the queue.
      * A LOCK can be applied to make the message not visible during this action until the lock is released
    * Receivers can PEEK a message, that is, examine without removing and decide if they retrieve the message for processing based on custom criteria.
      * A LOCK can be applied to make the message not visible during this action until the lock is released

* Quque-bases Asynchronous Messaging patterns: Competing Consumers
  * Multiple concurrent consumers to process messages received on the same messaging channel
  * A system can process multiple messages concurrently to optimize throughput, to improve reliability, scalability, availability and to optimize costs
  * Cuándo usar este patrón?
    * Tasks are independent and can run in parallel
    * Volume of work is highly variable requiring scalability
  * Y Cuándo no?
    * Tasks must be performed synchronously, and the application logic must wait for a task to complete before continuing
    * Tasks must be performed in a specific sequence

* Quque-bases Asynchronous Messaging patterns: Queue-based Load Leveling
  * Use a queue that acts as a buffer between a task and a service it invokes in order to smooth intermittent heavy loads that can cause the service to fail or the task to time out
  * This can help to minimize the impact of peaks in demand on availability and responsiveness for both the task and the service
  * Cuándo se usaría este patrón?
    * This pattern is useful to any application that uses services that are subject to overloading

* Quque-bases Asynchronous Messaging patterns: Publish & Subscribe

....
Dejé de apuntar, muchísimo contenido útil


#### Top Messaging Design patterns

...

#### Top Resilient Design patterns

...
