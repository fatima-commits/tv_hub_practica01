# TV Hub – Watch Experience

Práctica del curso **Tecnologías de desarrollo en el servidor**. Completé el flujo MVC necesario para que un usuario seleccione un canal en Home, la aplicación lo consulte mediante una API de Express, MongoDB devuelva su información y la página **Watch** reproduzca el stream con **Shaka Player**.

**Autor:** Fátima Vélez González

## Cómo ejecutar el proyecto

```bash
npm install
docker compose up -d
npm run build
npm run import:channels -- docs/japon_playlist.m3u Japan
npm run dev
```

1. `npm install` instala las dependencias.
2. `docker compose up -d` levanta MongoDB.
3. `npm run build` compila el proyecto TypeScript.
4. `npm run import:channels -- <playlist> <país>` importa los canales de una playlist a MongoDB.
5. `npm run dev` inicia el servidor. Después de iniciar sesión, Home debe mostrar los canales importados.

![Home con canales importados](docs/screenshots/00-home.png)

---

## El flujo que completé

```
Home (el usuario selecciona un canal)
  → watch.html + watch.js
  → GET /api/channels/:id
  → channel.routes.ts        (Route)
  → channel.controller.ts    (Controller)
  → Channel Model            (Mongoose)
  → MongoDB
  → respuesta JSON { channel }
  → watch.js (View)
  → Shaka Player
  → Stream
```

Trabajé capa por capa y comprobé cada una antes de pasar a la siguiente: primero la API, después la View y al final el player.

---

## Validación final

| Prueba | Resultado esperado | Resultado |
|---|---|---|
| 1. Iniciar sesión | Home muestra los canales importados | ✅ |
| 2. Seleccionar un canal | Se abre Watch con el identificador del canal | ✅ |
| 3. Revisar Network | Existe `GET /api/channels/:id` con respuesta 200 | ✅ |
| 4. Revisar la información | Nombre, país y categorías corresponden al canal | ✅ |
| 5. Stream disponible | La UI pasa de Loading a Playing | ✅ |
| 6. Stream no disponible | La UI muestra Error y Try again | ✅ |

---