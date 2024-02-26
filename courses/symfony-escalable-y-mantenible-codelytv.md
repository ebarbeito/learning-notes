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
          // ...
          ApiExceptionsHttpStatusCodeMapping $exceptionHandler
      ) {
          each(
              fn(int $httpCode, string $exceptionClass) => $exceptionHandler->register($exceptionClass, $httpCode),
              $this->exceptions()
          );
      }
  
      // ...
  
      abstract protected function exceptions(): array;
  }
  
  final class CoursesCounterGetController extends ApiController
  {
      public function __invoke(): JsonResponse
      { /* ... */ }
  
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
  
      // ...
  
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
  
          // ...
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

### Otras optimizaciones

#### Parsear JSON del request body

* La solución que se explica es interesante siempre que los argumentos de entrada vengan todos en el body de la petición HTTP (nota: aunque no está limitado al body, y podrían leerse cabeceras o cualquier información pública del objeto de la request)

* El método invocado del controlador recibe un argumento con un DTO ya instanciado (resuelto y deserializado) con los datos de la petición, sin necesidad de tratar el objecto Http Request

* Se hace uso de Symfony [Argument Resolver](https://symfonycasts.com/screencast/deep-dive/argument-resolver)

  ```php
  final class RegisterUserPutController
  {
      public function __invoke(RegisterUserCommand $command): JsonResponse
      {
  // ...
  ```

  ```php
  use CodelyTv\Shared\Domain\Bus\Command\Command;
  use Symfony\Component\HttpFoundation\Request;
  use Symfony\Component\HttpKernel\Controller\ArgumentValueResolverInterface;
  use Symfony\Component\Serializer\SerializerInterface;
  // use ...
  
  final class CommandValueResolver implements ArgumentValueResolverInterface
  {
      public function __construct(private SerializerInterface $serializer)
      {
      }
      
      public function supports(Request $request, ArgumentMetadata $argument): bool
      {
          return is_subclass_of($argument->getType(), Command::class);
      }
  
      public function resolve(Request $request, ArgumentMetadata $argument): Generator
      {
          yield $this->serializer->deserialize($request->getContent(), $argument->getType(), 'json');
      }
  }
  ```

#### Serializar respuestas automáticamente a JSON

* En lugar de devolver un objeto Http Response, se devuelve un DTO de la capa de aplicación que será serializado a JSON por Symfony

  ```php
  final class CourseCounterGetController
  {
      public function __invoke(): CoursesCounterResponse
      {
          // ...
          return new CoursesCounterResponse(10);
      }
  }
  ```

* Implementar un EventListener que escuche al evento KernelView de Symfony

  ```php
  // ...
  public function onKernelView(ViewEvent $event)
  {
     if (!$event->getControllerResult() instanceof Response) { return; }
  
     $event->setResponse(
         new JsonResponse(
             $this->serializer->serialize($event->getControllerResult(), 'json')
         )
     );
  }
  // ...
  ```

#### Añadir headers de forma global

* Si todas las respuestas tienen una o varias cabeceras que son siempre las mismas (e.g. CORS, o caché)

* Se puede hacer también vía Symfony Event Listener

  ```php
  // ...
  public function onKernelResponse(ResponseEvent $event)
  {
     $response = $event->getResponse();
     $response->setMaxAge(180);
     $response->setPublic();
  }
  // ...
  ```

#### Feature flags y Dark Launching

* Tener funcionalidades a producción donde solo ciertos usuarios pueden verlas (dark launching), o que las podamos activar sin necesidad de deployar código (feature flag / toggle)

* Una opción que se ve es para dark launching vía el evento de Symfony Kernel Request

  ```php
  // ...
  public function onKernelRequest(RequestEvent $event)
  {
  		// lógica para determinar si el usuario puede ver la funcionalidad
  
  		// en caso negativo, podemos enviarle una respuesta 404 Not Found
      $event->setResponse($response);
  }
  // ...
  ```

#### Progressive Rollout y A/B testing

* Caso de tener 2 controladores, uno nuevo y otro el actual para un mismo caso, y redirigir peticiones a uno u otro. Se propone varias alternativas

* Modificar el valor de _controller

  * El componente kernel de Symfony define un listener para reaccionar ante una request, es el `kernel.request`. Aquí añade la referencia al controlador en el atributo  `_controller`

    ```php
    // ...
    public function onKernelRequest(RequestEvent $event)
    {
        // lógica para determinar qué controlador usar

        $event->getRequest()->attributes->set('_controller', $controller);
    }
    // ...
    ```

  * Con un EventListener propio se puede modificar esta referencia. Hay que tener en cuenta las prioridades de llamada de los event listener, y usar una prioridad más baja que la usada por Symfony

    ```sh
    $ bin/console debug:event-dispatcher kernel.request
    
    Registered Listeners for "kernel.request" Event
    ===============================================
    
     ------- --------------------------------------------------------------------------------------- ----------
      Order   Callable                                                                                Priority
     ------- --------------------------------------------------------------------------------------- ----------
      #1      Symfony\Component\HttpKernel\EventListener\DebugHandlersListener::configure()           2048
      #2      Symfony\Component\HttpKernel\EventListener\ValidateRequestListener::onKernelRequest()   256
      #3      Symfony\Component\HttpKernel\EventListener\SessionListener::onKernelRequest()           128
      #4      Symfony\Component\HttpKernel\EventListener\LocaleListener::setDefaultLocale()           100
      #5      Symfony\Component\HttpKernel\EventListener\RouterListener::onKernelRequest()            32
      #6      Symfony\Component\HttpKernel\EventListener\LocaleListener::onKernelRequest()            16
     ------- --------------------------------------------------------------------------------------- ----------
    ```

* Reaccionar al evento Controller

  * Settear el método de llamada del controlador justo antes de que se llame al mismo

    ```php
    // ...
    public function onKernelController(ControllerEvent $event)
    {
        // ...
        $event->setController($controllerCallable);
    }
    // ...
    ```

* Implementar un ControllerResolver

  * Crear un controller resolver propio y establecer en él el controlador a llamar
  * Algunas referencias: [1](https://symfony.com/doc/current/create_framework/http_kernel_controller_resolver.html), [2](https://symfonycasts.com/screencast/deep-dive/controller-resolver)


#### Poner la web en estado de mantenimiento

* Una opción es reaccionando de nuevo a `kernel.request`

  ```php
  public function onKernelRequest(RequestEvent $event)
  {
      // ...
      $response->setStatusCode(Response::HTTP_SERVICE_UNAVAILABLE)
      $event->setResponse($response);
  }
  ```
## Persistencia con Doctrine

### ORM, Dbal, SQL ¿Cuándo usar cada uno?

* Si se elige usar ORM, tener siempre en cuenta que las SQL queries que se ejecutarán finalmente van a depender mucho de cómo están modeladas las entidades implicadas. Se puede usar el Symfony profiler para analizar estas queries y ver si se pueden mejorar en caso de ser necesario
* Siempre binding de valores a parámetros tanto con PDO como con Dbal para evitar SQL injection

### Streaming de datos: procesar archivo y enviar respuesta HTTP

* [Symfony\Component\HttpFoundation\StreamedResponse](https://symfony.com/doc/current/components/http_foundation.html#streaming-a-response)
* StreamedResponse allows to serve small chunks of data to the client

```php
$response = new StreamedResponse();
$response->setCallback(function () use ($fileHandle) {
    while (!feof($fileHandle)) {
        if (!$line = fgets($fileHandle)) {
            break;
        }
				echo $line; // sends data
    }

    fclose($fileHandle);
});
$response->headers->set('Content-Type', 'text/plain');
```

* It uses a function callback where is possible to iterate over a set of big data and stream it in small chunks
* Next example is using Doctrine to stream data from the database

```php
$response->setCallback(function() {
    $query = $this->entityManager->createQuery("SELECT f FROM App\Entity\Food f ORDER BY f.id DESC");
    /** @var Food $food */
    foreach ($query->toIterable() as $key => $food) {
        echo $food->id() . ' ' . $food->name() . PHP_EOL;

        $this->entityManager->clear($food);
    }
});
$response->headers->set('Content-Type', 'text/plain');
```

* Doctrine allows it, and if the dbms you are using allows it as well and you have it correctly configured, then the data can be sent streamed to the HTTP responsedoc
* `toIterable()` facility to iterate over the query results step by step instead of loading the whole result into memory at once — [Doctrine Batch Processing, iterating results](https://www.doctrine-project.org/projects/doctrine-orm/en/3.0/reference/batch-processing.html#iterating-results)
* Another reference: [Streaming Files in Symfony](https://www.slingacademy.com/article/streaming-files-in-symfony-complete-guide-examples/)

### Streaming de datos y procesos en batch con Doctrine

* [Doctrine Batch Processing](https://www.doctrine-project.org/projects/doctrine-orm/en/current/reference/batch-processing.html)
