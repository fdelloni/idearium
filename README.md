# Idearium · v0.1

Banco de proyectos / ideas para el grupo. Página estática (HTML + JSON), sin servidor.

## Cómo abrirla

La página NO funciona con doble clic (`file://`) porque carga `data.json` con `fetch`, y los navegadores lo bloquean en ese modo. Hay que servirla con un servidor local. Tres alternativas, de más simple a menos:

1. **Con Node** (si lo tenés instalado):
   ```
   npx serve .
   ```
   Abre `http://localhost:3000` en el navegador.

2. **Con Python** (viene en muchas computadoras):
   ```
   python -m http.server 8000
   ```
   Abre `http://localhost:8000`.

3. **Subirla a un hosting estático**: Vercel, Netlify, GitHub Pages, Cloudflare Pages. Todos gratuitos. La página tiene `<meta name="robots" content="noindex">` así que Google no la indexa, pero igualmente conviene mantenerla en una URL no compartible públicamente.

## Contraseña

Por defecto la contraseña es `password`. **Cambiala antes de compartir la página.** En `data.json`, dentro de `config`, está el campo `passwordHash` — es un hash SHA-256 de la contraseña en minúsculas.

Para generar un hash nuevo:

- Online: cualquier herramienta de "SHA-256 generator" — pegás la contraseña, te devuelve el hash.
- Desde la consola del navegador (F12 → Console):
  ```js
  crypto.subtle.digest('SHA-256', new TextEncoder().encode('TU_PASSWORD'))
    .then(b => console.log(Array.from(new Uint8Array(b)).map(x => x.toString(16).padStart(2,'0')).join('')))
  ```

Importante: el gate por contraseña client-side **no es seguridad real**. Cualquier persona con conocimientos básicos puede inspeccionar el código y leer `data.json`. Es solo una barrera para evitar accesos casuales y filtrar bots. Para seguridad real, hay que migrar a un backend con autenticación.

## Cómo agregar una idea

Cuando un miembro proponga una idea desde el botón "+ Proponer mi idea", te llegará un mensaje formateado por WhatsApp. Tomás esos datos y agregás un nuevo objeto al array `ideas` en `data.json` con esta estructura:

```json
{
  "id": "idea-XXXX",
  "miembro": "Nombre Apellido",
  "numeroSocio": "M-0123",
  "fechaCarga": "2026-05-01",
  "estado": "idea | exploracion | buscando socios | en marcha",
  "rubro": "tecnologia | salud | educacion | finanzas | industria | energia | servicios | impacto social | otros",
  "titulo": "Una línea que resume la idea",
  "empresa": "Nombre de empresa o null",
  "descripcion": "Descripción más larga del proyecto",
  "buscando": ["co-founder técnico", "inversor", "feedback"],
  "telefono": "+54 9 11 0000 0000",
  "compartirTelefono": true
}
```

- `compartirTelefono: true` → en la tarjeta aparece botón "WhatsApp directo" al teléfono del autor.
- `compartirTelefono: false` → aparece "Contactar vía [admin]" y el WhatsApp lleva al admin con un mensaje pre-armado.

## Configuración del admin

En `data.json`, dentro de `config`:

- `nombreProducto`: el nombre que se ve arriba a la izquierda. Si decidís cambiar el branding, va acá.
- `subtitulo`: bajada chica.
- `adminWhatsApp`: tu número en formato internacional sin `+`, sin espacios. Por ejemplo `5491133334444`. Es a donde se envían las propuestas y los contactos "vía admin".
- `adminNombre`: lo que se muestra en los botones "Contactar vía X".
- `passwordHash`: el SHA-256 del password.
- `passwordHint`: un texto opcional que se muestra debajo del input de contraseña. Útil al principio para que sepan qué password usar — borralo cuando ya no haga falta.

## Próximos pasos sugeridos

- Migrar a una base real (Supabase, Airtable) cuando haya volumen.
- Agregar campo "fecha de última actualización" para que las ideas no se queden obsoletas.
- Sumar un sistema de comentarios o reacciones entre miembros.
- Versión móvil más cuidada (la actual es responsive básica).
- Newsletter mensual con las ideas nuevas.

## Sobre el nombre

"Idearium" es propuesta del autor del proyecto, no es una marca registrada vinculada a Mensa Argentina. Si la asociación quiere usar la marca propia, conviene consultar primero con la directiva por las políticas internas de uso del nombre.
