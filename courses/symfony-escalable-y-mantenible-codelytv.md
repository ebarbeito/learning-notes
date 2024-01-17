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

