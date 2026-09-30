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

## Tarea 11 — Otros recursos

Además de /posts, probé los recursos:

- /users
- /comments

También probé la ruta:

/posts/1/comments

La estructura de esta URL indica que se están solicitando los comentarios asociados a la publicación cuyo ID es 1. Deducí la estructura porque /posts identifica el recurso principal, /1 identifica una publicación específica y /comments representa un recurso relacionado con esa publicación.

## Tarea 13 — Pruebas automáticas

### Prueba 1 — Estado HTTP

```javascript
pm.test("El estado es 200", function () {
    pm.response.to.have.status(200);
});