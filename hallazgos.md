# Hallazgos

## Tarea 7 — Diferencia entre PUT y PATCH

### Respuesta PUT

{
    "title": "Título modificado",
    "id": 1
}

### Respuesta PATCH

{
    "userId": 1,
    "id": 1,
    "title": "Título modificado",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}


## Tarea 10 — Límite de publicaciones

Después de realizar pruebas incrementando el ID, encontré que:

- El ID más alto que devuelve 200 es: [100]
- El primer ID que devuelve 404 es: [101]

Este tipo de prueba se conoce como análisis de valores límite (Boundary Value Analysis). Se utiliza porque muchos errores aparecen precisamente en los valores cercanos a los límites permitidos por un sistema.