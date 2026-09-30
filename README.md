## Marco conceptual

### ¿Qué es una API REST?

Una API REST es una forma de comunicación entre diferentes aplicaciones o sistemas utilizando principalmente el protocolo HTTP. REST significa *Representational State Transfer* y establece una serie de principios para organizar la comunicación entre un cliente y un servidor. En este tipo de arquitectura se trabaja con recursos, por ejemplo usuarios, productos o publicaciones, y cada recurso puede ser identificado mediante una dirección o endpoint. Un endpoint es la ruta específica que permite acceder a un recurso o realizar una determinada operación sobre él. Un ejemplo de una aplicación que depende de APIs es WhatsApp, ya que necesita comunicarse con servidores para enviar y recibir información entre los usuarios. En este taller se utilizará JSONPlaceholder, donde un ejemplo de endpoint es `/posts/1`, utilizado para consultar una publicación específica.

**Fuente consultada:** MDN Web Docs — REST: https://developer.mozilla.org/es/docs/Glossary/REST


## Métodos HTTP

| Método HTTP | Operación CRUD | Qué hace                                                                            |
| ----------- | -------------- | ----------------------------------------------------------------------------------- |
| GET         | Read           | Consulta o recupera información de un recurso.                                      |
| POST        | Create         | Envía información al servidor y normalmente se utiliza para crear un nuevo recurso. |
| PUT         | Update         | Actualiza o reemplaza completamente un recurso.                                     |
| PATCH       | Update         | Modifica parcialmente un recurso, cambiando solamente los campos necesarios.        |
| DELETE      | Delete         | Elimina un recurso determinado.                                                     |

La relación entre HTTP y CRUD me permite entender que los métodos HTTP indican qué tipo de operación quiero realizar sobre un recurso. Por ejemplo, con GET puedo consultar una publicación, con POST puedo crearla, con PUT o PATCH puedo modificarla y con DELETE puedo eliminarla.

**Fuentes consultadas:**
MDN Web Docs — Métodos HTTP: https://developer.mozilla.org/es/docs/Web/HTTP/Reference/Methods
MDN Web Docs — POST: https://developer.mozilla.org/es/docs/Web/HTTP/Reference/Methods/POST
MDN Web Docs — PUT: https://developer.mozilla.org/es/docs/Web/HTTP/Reference/Methods/PUT
MDN Web Docs — PATCH: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PATCH
MDN Web Docs — DELETE: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/DELETE


## Códigos de estado

Los códigos de estado HTTP sirven para indicar qué ocurrió después de realizar una solicitud. Se agrupan en cinco familias dependiendo del primer número del código.

| Familia | Significado                                               | Ejemplo                   |
| ------- | --------------------------------------------------------- | ------------------------- |
| 1xx     | Respuestas informativas                                   | 100 Continue              |
| 2xx     | Solicitudes procesadas correctamente                      | 200 OK                    |
| 3xx     | Redirecciones                                             | 301 Moved Permanently     |
| 4xx     | Errores relacionados con la solicitud del cliente         | 404 Not Found             |
| 5xx     | Errores ocurridos al procesar la solicitud en el servidor | 500 Internal Server Error |

La diferencia entre los códigos 4xx y 5xx es importante porque permiten identificar en qué parte está el problema. Los códigos 4xx normalmente indican que existe algún inconveniente con la solicitud realizada por el cliente, como pedir un recurso que no existe. Por ejemplo, un 404 significa que no se encontró el recurso solicitado. En cambio, los códigos 5xx indican que el servidor tuvo un problema al intentar procesar la solicitud. Por ejemplo, un 500 significa que el servidor encontró una situación que no pudo manejar correctamente.

Por eso no todos los códigos diferentes de 200 significan que una prueba falló. Lo importante es comparar el resultado obtenido con el resultado que se esperaba para ese caso de prueba.

**Fuente consultada:** MDN Web Docs — Códigos de estado HTTP: https://developer.mozilla.org/es/docs/Web/HTTP/Reference/Status


### Diferencia entre consultar un recurso y consultar una colección

Cuando consulto un recurso específico, como `/posts/1`, el criterio principal es comprobar que ese recurso exista, que tenga la información esperada y que la respuesta tenga el código de estado correcto. En este caso espero recibir un solo objeto con campos como `userId`, `id`, `title` y `body`.

En cambio, cuando consulto una colección como `/posts`, además de comprobar el código de estado, debo verificar que la respuesta sea un arreglo y que contenga los elementos esperados. También puedo revisar cuántos elementos devuelve y si cada elemento mantiene la estructura correcta.

Por lo tanto, la diferencia principal es que en un recurso compruebo un elemento específico, mientras que en una colección tengo que comprobar también la cantidad y estructura de los elementos que forman parte de ella.


### Tarea 5 — Provocar un error

La petición `GET /posts/9999` produjo un código `404 Not Found`, porque se está solicitando un recurso que no existe. Considero que el caso de prueba pasó porque antes de realizar la petición había establecido que esperaba un código 404 y el resultado obtenido fue exactamente ese.

Esto me permitió entender que un caso de prueba no se considera fallido simplemente porque la respuesta tenga un código diferente de 200. Lo importante es comparar el resultado obtenido con el resultado esperado. Si esperaba un 404 y recibí un 404, la prueba pasó.

Si la misma petición hubiera devuelto un código 200 con el cuerpo vacío, el caso de prueba fallaría, porque el comportamiento esperado para un recurso inexistente era recibir un 404. Esto mostraría una diferencia entre el comportamiento esperado y el comportamiento obtenido.


### Tarea 6 — Crear un recurso con POST
Al ejecutar la petición POST varias veces observé que la API respondía correctamente y devolvía un recurso con un identificador. En JSONPlaceholder el identificador devuelto para esta prueba se mantiene en 101, aunque estoy enviando la misma información varias veces. Esto ocurre porque JSONPlaceholder es una API diseñada principalmente para realizar pruebas y simulaciones, por lo que las operaciones de escritura no representan una creación permanente en una base de datos real.
En una API real comprobaría la creación consultando posteriormente el recurso mediante una petición GET utilizando el ID recibido. También verificaría que los datos enviados coincidan con los datos almacenados. Otra posibilidad sería revisar la base de datos o utilizar otro endpoint de consulta que confirme que el recurso existe.


### Tarea 7 — La diferencia entre PUT y PATCH
Para corregir un error de escritura en un solo campo utilizaría PATCH, porque permite modificar parcialmente el recurso sin tener que enviar nuevamente todos sus campos. PUT normalmente se utiliza cuando se quiere reemplazar la representación completa del recurso.


### Tarea 8: Idempotencia


## ¿Qué significa idempotencia?

En palabras sencillas:

> Un método es idempotente cuando realizar la misma operación varias veces produce el mismo efecto final que realizarla una sola vez.

Por ejemplo, si hacemos:

```text
PUT → modificar publicación 1