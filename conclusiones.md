## Tarea 8 — Idempotencia

La idempotencia significa que ejecutar varias veces la misma operación produce el mismo efecto final que ejecutarla una sola vez.

Al probar PUT varias veces observé que la modificación se mantiene en el mismo estado. En cambio, POST representa una operación de creación, por lo que repetirla puede generar nuevos recursos.

Los métodos HTTP que normalmente se consideran idempotentes son GET, PUT y DELETE. PATCH puede ser idempotente dependiendo de la operación que se implemente, mientras que POST no es idempotente.

La prueba en Postman permitió comprobar la diferencia entre repetir una modificación y repetir una operación de creación.