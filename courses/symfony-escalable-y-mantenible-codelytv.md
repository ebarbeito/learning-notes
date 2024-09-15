# Symfony mantenible y escalable

by Dani Santamaría, Jabvier Ferrer – CodelyTV

------

> Aprende herramientas y prácticas con Symfony para conseguir una mayor mantenibilidad y escalabilidad de tus aplicaciones.

**Available resources**

-  [Course materials](https://pro.codely.tv/library/symfony-mantenible-y-escalable-127478)
-  [GIt reposiroty](https://github.com/CodelyTV/symfony-maintainable-scalable-course)

🏷️ Tags: `course`, `2021`, `codelytv`, `symfony`, `backend`, ...

------

## Introducción, historia, filosofía, arquitectura Symfony

* Se dan notas históritcas sobre Symfony. Aquí unas notas
* Uso del patrón DataMapper reemplazando al ActiveRecord utilizado inicialmente
* Symfony Flex
* Definición de servicios vía autowiring
* Parametrización vía variables de entorno
* Symfony trata de segregar interfaces con propósitos muy concretos, permitiendo que los clientes puedan usarlas y expandirlas si quieren. Un ejemplo es `\Symfony\Component\HttpKernel\Kernel`
* Componentes Symfony EventDispatcher y Symfony Messenger
* Iteración del framework para permitir difinición de todo como servicios, incluso los controladores. Era típico el buscar cómo definir controladores como servicios
* Uso de event dispatchers de Symfony y middleware de Laravel

![Event dispatcher and middleware](.assets/symfony-escalable-y-mantenible-codelytv.md/event_dispatcher_and_middleware.png)
([Referencia](https://www.researchgate.net/figure/Event-dispatcher-and-middleware_fig2_330656531))

* Notas de arquitectura
  * Uso de patrón [Front Controller](https://martinfowler.com/eaaCatalog/frontController.html) para manejar todas las peticiones partiendo del mismo punto
  * La página del curso tiene un [resumen](https://pro.codely.com/library/symfony-mantenible-y-escalable-127478/308903/path/step/129421021/) del flujo por escrito
  * Componentes principales: [HttpFoundation](https://symfony.com/components/HttpFoundation), [HttpKernel](https://symfony.com/doc/current/components/http_kernel.html), [EventDispatcher](https://symfony.com/doc/current/components/event_dispatcher.html)


![Symfony request handling](.assets/symfony-escalable-y-mantenible-codelytv.md/symfony_request_handling.png)

## Migración progresiva del Legacy a Symfony

* La clave del proceso es hacerlo progresivo. No tratar de reemplazar el sistema legacy por el nuevo de un solo golpe, sino ir haciendo cambios y despliegues progresivos
* Explican 2 posibles estrategias para ir reemplazando el legacy por Symfony [*sidenote*: <unque siempre hay más alternativas. Elegir una u otra puede depender de qué tipo de legacy hhay que migrar o de su complejidad]

### Fallback al front controller Legacy

* Se crea un nuevo proyecto Symfony totalmente separado del legacy
* El front controller de Symfony es el punto de entrada a la aplicación (`public/index.php`)
* A grandes rasgos, se modifica el front controller de tal modo que todas aquellas rutas que dan una respuesta 404, se ejecuta el controlador legacy correspondiente para que maneje la petición
* El controlador legacy actuaría de fallback en este caso

### Cargar rutas Legacy

* Aquí se añaden las rutas legacy a la instancia de RouteCollection que Symfony crea en base a la configuración de rutas del nuevo proyecto
* Para las rutas legacy, se crea un nuevo custom Symfony Loader que [se etiqueta](https://symfony.com/doc/current/routing/custom_route_loader.html#creating-a-custom-loader) en el inyector de dependencias como tal para que sea cargado
* En este custom Loader, es donde se crea el nuevo controlador para manejar las rutas legacy, asociando estas rutas legacy a un nuevo controlador manejado en la aplicación de Symfony
* En este controlador es donde se maneja la ejecución del controlador legacy para cuando esta ruta no tenga todavía un reemplazo en la configuración de rutas del proyecto
* Aquí ya no hablamos de fallback, pues las rutas legacy son ahora manejadas como rutas del proyecto Symfony al mismo nivel (e.g. se pueden ver utilzando el comando `debug:router`)

## Configurar y adaptar Symfony para mejorar la mantenibilidad

* Detalles sobre el Contenedor de Inyección de Dependencias de Symfony ([componente](https://symfony.com/doc/current/components/dependency_injection.html))
* Creación de distintos Kernels para distintos front controllers para distintas aplicaciones desarrolladas en un único monorepo
* Uso del autoconfigure de Symfony y la opción `_instanceof`del contenedor para tanguear servicios delegando en Symfony sin tener que hacerlo manualmente. [Autoconfiguring tags](https://symfony.com/doc/current/service_container/tags.html#autoconfiguring-tags) y [Reference tagged services](https://symfony.com/doc/current/service_container/tags.html#reference-tagged-services)

## Optimizaciones habituales en peticiones HTTP

### Gestión de errores

* Control de una excepción no capturada transformándola en una respuesta controlada con un determinado HTTP status code

* Cada vez que en un controlador hay que añadir el control de una nueva excepción, hay que modificar la clase del controlador. Si se quiere aplicar SOLID aquí, se podría no modificar la clase y controlar las excepciones de todos los controladores en un punto común

* Varias formas de hacerlo. La manera propuesta

  ```php
  abstract class ApiController
  {
      public function __construct(
          // (...)
          ApiExceptionsHttpStatusCodeMapping $exceptionHandler
      ) {
          each(
              fn(int $httpCode, string $exceptionClass) => $exceptionHandler->register($exceptionClass, $httpCode),
              $this->exceptions()
          );
      }

      // (...)

      abstract protected function exceptions(): array;
  }

  final class CoursesCounterGetController extends ApiController
  {
      public function __invoke(): JsonResponse
      { /* (...) */ }

      protected function exceptions(): array
      {
          return [
              CoursesCounterNotExist::class => Response::HTTP_NOT_FOUND,
              // ...
          ];
      }
  }
  ```

  ```php
  final class ApiExceptionsHttpStatusCodeMapping
  {
      private const DEFAULT_STATUS_CODE = Response::HTTP_INTERNAL_SERVER_ERROR;

      // (...)

      public function statusCodeFor(string $exceptionClass): int
      {
          return get(
              key: $exceptionClass,
              collection: $this->exceptions,
              default: self::DEFAULT_STATUS_CODE
          );
      }
  }

  final class ApiExceptionListener
  {
      public function __construct(private ApiExceptionsHttpStatusCodeMapping $exceptionHandler)
      {
      }

      public function onException(ExceptionEvent $event): void
      {
          $exception = $event->getThrowable();

          $event->setResponse(new JsonResponse(
              [
                  'code'    => $this->exceptionCodeFor($exception),
                  'message' => $exception->getMessage(),
              ],
              $this->exceptionHandler->statusCodeFor($exception::class)
          ));

          // (...)
      }
  }
  ```

  **ApiController**: Clase abstracta que usa el patron [template method](https://refactoring.guru/design-patterns/template-method) en el método exceptions para que los controladores definan el un diccionario que mapee la excepción con un código de respuesta HTTP

  **ApiExceptionsHttpStatusCodeMapping**: Maneja el mapeo de códigos

  **ApiExceptionListener**: Symfony EventListener donde se hace uso de la clase anterior; reacciona a las excepciones para construir una respuesta de error HTTP

### Optimizar el rendimiento

* Ejemplo de enviar un email tras devolver la respuesta

### **Procesado de eventos de dominio en Event Subscriber**

* Hacer uso del `kernel.terminate` para todo lo que no sea nbecesario procesar para darle la respuesta al usuario
