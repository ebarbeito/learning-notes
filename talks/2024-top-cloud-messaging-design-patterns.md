# Top Cloud Messaging Design Patterns

José Antonio Muro - Centro de Novas Tecnoloxías de Galicia

------

**Available resources**

- [Xornadas CNTG](https://cntg.xunta.gal/web/cntg/xornadas/-/detalle/693202)

🏷️ Tags: `talk`, `2024`, `cloud`, `architecture`, `infrastructure`, `patterns`

------

## General notes (mostly Spanish)

### Falacias dentro de la computación distribuida

* La red es fiable. Falacia: no podemos asumir que la red es fiable ni que siempre estará disponible.
  * Timeout, circuit breaker, diseño basado en colas, ...
  * Para mitigar este problema:
    * Patrones de retry, pero con precaución
    * Protegernos de sistemas externos
    * Patrón de *circuit breaker*: si se alcanza cierto número de reintentos, se abre el circuito (se abandona el mecanismo de reintentos)
* La latencia es cero. En sistemas distribuidos, la latencia nunca es nula.
  * Considerar la latencia cuando se realizan llamadas concatenadas a microservicios.
  * Evitar llamadas innecesarias -> usar caché.
  * Para mitigar este problema:
    * Estrategias de caché como *cache-aside*, solicitudes masivas, alojamiento de contenido estático cerca de los clientes.
* Ancho de banda infinito. No lo es, ni tampoco lo es la capacidad de computación.
  * Para mitigar este problema:
    * Políticas de *throttling*.
    * Small payloads such as claim-check patterns
      * Mensajes con menos información, con URI para recuperar la información más pesada: reduce el tamaño de los mensajes, optimiza recursos, y recupera la información pesada solo cuando se necesite (uso explícito de la URI).
* La topología no cambia. Nada está exento de cambios.
  * Para mitigar este problema:
    * Herramientas de descubrimiento de servicios.
    * No codificar IPs directamente.
    * Offload cross-curring concerns
* Asumir que siempre hay un SysOp que se encargue de las operaciones. No siempre es así.
  * Para mitigar este problema:
    * Adoptar DevOps.
    * Reducir la dependencia del *Bus Factor*.
      * Este es el riesgo de que algo le suceda al SysOp (por ejemplo, un accidente).
      * No depender de personas específicas, distribuir el conocimiento.
* El coste de transporte es cero.
  * Para mitigar este problema:
    * Cálculo de costes.
    * Elegir el mejor protocolo para tu caso de uso, como JSON, gRPC, etc.
* El versionado de componentes es simple. Posteriormente se demuestra que no fue tan sencillo...
  * Para mitigar este problema:
    * Adoptar una cultura de optimización continua.
    * Conocer tu plataforma.
    * Centrarte en los componentes clave y almacenar registros útiles.
    * Automatizar alertas ante eventos críticos.
    * Crear formatos estándar para el registro de datos.
    * Asegurar que los datos puedan ser agregados y centralizados.

### Pilares de la nube y patrones de diseño

#### Fiabilidad

La capacidad de un sistema para recuperarse de fallos y seguir funcionando correctamente.

Patrones de diseño:

* **Design for self-healing**
  * Apply retry, circuit breaker, bulkhead, load-leveling, throttling patterns, etc.
* **Aplicar redundancia**
  * Utilizar replicación, health probes, mecanismos automáticos de descubrimiento de servicios para conectividad dinámica, automatic failovers, etc.

#### Excelencia Operacional

Procesos operativos que mantienen un sistema funcionando en producción sin problemas ni interrupciones.

(En resumen, aplicar la cultura DevOps)

Patrones de diseño:

* **Establecer estándares de desarrollo (DevOps)**
  * Aplicar solid principles, políticas de código de calidad, arquitecturas limpias, offload cross-cutting concerns, usar arquitecturas basadas en eventos, etc.
* **Diseñar para la operación (DevOps)**
  * Automatizar para la eficiencia (CI/CD, laC), adoptar prácticas de despliegue seguro, evolucionar las operaciones con observabilidad, pruebas, monitoreo y versionado.

#### Eficiencia en el rendimiento

La capacidad de un sistema para adaptarse a cambios en la carga sin perder rendimiento.

* **Negociar objetivos de rendimiento realistas**
  * Esforzarse por comprender completamente tu caso de uso y colaborar con los propietarios del negocio.
* **Diseñar para cumplir con los requisitos de capacidad elástica**
  * Escalar hacia fuera a medida que aumenta la carga y escalar hacia dentro cuando no se necesite capacidad adicional.
* **Usar el mejor almacenamiento de datos según tu escenario**
  * Adoptar escenarios de persistencia poliglota, especialmente en aplicaciones distribuidas o orientadas a microservicios.
* **Optimización continua**
  * Fomentar una cultura de optimización impulsada por el rendimiento, monitorear proactivamente los patrones de rendimiento y ajustar las aplicaciones.

Patrones de diseño:

* Asynchronous request-reply
* Cache-aside
* Bulkhead
* Claim check
* CQRS
* Tabla de índices
* Vista materializada
* *Sharding*
* *Sidecar*
* Alojamiento de contenido estático
* etc.

#### Optimización de costes

Gestionar los costes para maximizar el valor entregado alineado con las necesidades empresariales y los presupuestos.

Patrones de diseño:

* **Optimizar según las necesidades empresariales y los costes**
  * Alinear el diseño con las necesidades empresariales y los costes considerando SLAs, RTOS, RPOs, etc.
* **Diseñar para escalar hacia fuera / escalar hacia dentro**
  * Escalar hacia fuera a medida que aumenta la carga y escalar hacia dentro cuando no se necesite capacidad adicional.

Patrones de diseño:

* Claim check
* Competing consumers
* Publisher/subscriber
* Queue-based load leveling
* Throttling
* etc.

#### Seguridad

Proteger las aplicaciones y los datos de amenazas.

### Estilos de arquitectura

#### N-Tiers

* **Arquitectura N-Tier**. Divide una aplicación en capas lógicas y niveles físicos.

  * **Layers** son una forma lógica de reflejar la arquitectura interna, separando responsabilidades y gestionando las dependencias entre componentes de software.
  * **Tiers** son una forma física de separar las capas, generalmente corriendo en máquinas separadas.

Cuándo usarla:

* Aplicaciones web simples o aplicaciones tradicionales locales.
* Se necesita una portabilidad sencilla entre la nube y el entorno local, y entre plataformas de nube con un mínimo de refactorización.

#### Web Queue Worker

* Una arquitectura *Web Queue Worker* generalmente se compone de:
  * Una aplicación web o API web que atiende las solicitudes de los clientes.
  * Un trabajador que realiza tareas intensivas en recursos, flujos de trabajo largos o trabajos por lotes.
  * Una cola que proporciona comunicación asincrónica entre el frontend web y el trabajador.
* El frontend puede consistir en una API web. En el lado del cliente, la API web puede ser consumida por una aplicación de una sola página que realiza llamadas AJAX o por una aplicación nativa.

Cuándo usarla:

* Aplicaciones con dominios simples.
* Aplicaciones con flujos de trabajo largos o operaciones por lotes.
* Cuando se prefiera usar servicios gestionados en lugar de infraestructura como servicio (IaaS).

#### Event Driven

* Una arquitectura orientada a eventos se compone de productores de eventos que generan un flujo de eventos, y consumidores de eventos que escuchan esos eventos.
* Existen diferentes variantes de la arquitectura orientada a eventos:
  * **Pub/Sub**: Cuando se publica un evento, se envía a cada subscriber. Después de que un evento es recibido, no puede ser reproducido, y los nuevos subscribers no verán ese evento.
  * **Event Streaming**: Los eventos se escriben en un registro. Los eventos son estrictamente ordenados (dentro de una partición) y duraderos. Los clientes no se suscriben al flujo; en su lugar, un cliente puede leer desde cualquier parte del flujo. El cliente es responsable de avanzar en su posición en el flujo, lo que significa que puede unirse en cualquier momento y reproducir eventos.

Cuándo usarla:

* Múltiples subsistemas deben procesar los mismos eventos.
* Procesamiento en tiempo real con el menor retraso posible.
* Alto volumen y alta velocidad de datos, como IoT.
* Es necesario desacoplar productores y consumidores, pero proporcionando una integración asincrónica con consistencia eventual.

#### Microservicios

* Una arquitectura de microservicios consiste en una colección de servicios pequeños, independientes, autónomos, auto contenidos y desacoplados.
* Cada servicio es autónomo y debe implementar una única capacidad empresarial dentro de un contexto limitado, mejorando la escalabilidad y el aislamiento de fallos.
* Los servicios pueden desarrollarse y desplegarse de forma independiente, mejorando la agilidad. Un equipo puede actualizar un servicio existente sin necesidad de volver a desplegar toda la aplicación.
* Cada servicio es una base de código separada, que puede ser gestionada por un pequeño equipo de desarrollo, lo que mejora la productividad y reduce la sobrecarga de gestión.

Cuándo usarla:

* Cuando la capacidad de escalar componentes individuales de manera independiente sea esencial.
* Cuando se requieren equipos autónomos responsables de partes de la aplicación.
* Cuando se necesiten lanzamientos rápidos y actualizaciones frecuentes.

#### Serverless

* **Serverless** se refiere a una arquitectura sin servidores, donde los proveedores de la nube gestionan las infraestructuras y los recursos de computación, en lugar de gestionarlos uno mismo.
* Los recursos de cómputo se asignan bajo demanda, basados en los eventos que provocan las ejecuciones de funciones.
* En este modelo, los costos se basan en la cantidad de recursos utilizados en función de la carga, ya que no se paga por la infraestructura reservada.

Cuándo usarla:

* Ideal para aplicaciones con cargas variables.
* Para aplicaciones con alta variabilidad en la demanda.
* Para desarrollar rápidamente sin preocuparse por la infraestructura.
