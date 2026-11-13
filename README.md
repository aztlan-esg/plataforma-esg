# Plataforma ESG Aztlan — Guía de instalación

Plataforma ESG con la **Metodología propietaria Aztlan**. La página se publica en **GitHub Pages** y todos los datos y evidencias se guardan en el **SharePoint de Aztlan** (Microsoft 365). Los clientes entran con su propio correo como invitados.

Tiempo estimado: **45 minutos**, una sola vez.

---

## Paso 1 · Crear el sitio de SharePoint (5 min)

1. Entra a **https://www.office.com** → SharePoint → **+ Crear sitio** → **Sitio de equipo**.
2. Nombre: **Plataforma ESG** (la dirección quedará como `https://<tuempresa>.sharepoint.com/sites/PlataformaESG`).
3. Privacidad: **Privado**. Agrega como miembros solo a los consultores de Aztlan.
4. Permite invitar externos al sitio: **Centro de administración de SharePoint → Sitios activos → Plataforma ESG → Uso compartido → "Invitados nuevos y existentes"**. (Si no tienes acceso al centro de administración, pídeselo a quien administra Microsoft 365.)

> La plataforma crea sola las carpetas **Clientes**, **Revisiones** y **Publico** dentro de *Documentos* la primera vez que entras.

## Paso 2 · Registrar la aplicación en Microsoft Entra (15 min)

Lo hace un **administrador de Microsoft 365**.

1. Entra a **https://entra.microsoft.com** → **Aplicaciones → Registros de aplicaciones → + Nuevo registro**.
2. Nombre: **Plataforma ESG Aztlan**. Tipos de cuenta: **Solo las cuentas de este directorio organizativo**.
3. URI de redirección: elige **Aplicación de página única (SPA)** y escribe la dirección de GitHub Pages (Paso 3), por ejemplo `https://aztlan-mx.github.io/plataforma-esg/` *(con la diagonal final)*.
4. Copia el **Id. de aplicación (cliente)** y el **Id. de directorio (inquilino)**.
5. **Permisos de API → + Agregar un permiso → Microsoft Graph → Permisos delegados**: `User.Read`, `Files.ReadWrite.All`, `Sites.ReadWrite.All`.
6. Presiona **"Conceder consentimiento de administrador para …"** (así los clientes no ven pantallas de permisos).
7. Revisa que en **Identidades externas → Configuración de colaboración externa** esté permitido invitar invitados y activado el **código de acceso de un solo uso por correo** (para clientes con Gmail u otros correos).

## Paso 3 · Publicar en GitHub Pages (10 min)

1. Crea un repositorio en GitHub, por ejemplo **plataforma-esg** (puede ser público: no contiene datos ni contraseñas).
2. Sube todos los archivos de esta carpeta (`index.html`, `config.js`, `msal-browser.min.js`, `.nojekyll`, `README.md`).
3. **Settings → Pages → Build and deployment → Source: Deploy from a branch → main / (root) → Save**.
4. En 1–2 minutos la plataforma queda en `https://<usuario>.github.io/plataforma-esg/`.

## Paso 4 · Llenar `config.js` (5 min)

Edita `config.js` en GitHub (ícono de lápiz) y completa:

```js
TENANT_ID: 'el Id. de directorio (inquilino)',
CLIENT_ID: 'el Id. de aplicación (cliente)',
SP_HOST: 'tuempresa.sharepoint.com',
SP_SITE_PATH: '/sites/PlataformaESG',
DRIVE_ID: '',            // lo copias en el Paso 5
APP_URL: 'https://<usuario>.github.io/plataforma-esg/',
```

## Paso 5 · Primer ingreso (5 min)

1. Abre la plataforma e inicia sesión con tu cuenta de Aztlan. Entrarás como **administradora** (por ser miembro del sitio).
2. En **Cartera de clientes → Invitar cliente** verás el **DRIVE_ID**: cópialo a `config.js` (recomendado para que los clientes se conecten directo).

## Invitar a un cliente

**Cartera de clientes → Invitar cliente**: razón social, sector y correo(s). La plataforma:

- crea `Clientes/<empresa>` (el cliente puede editar) y `Revisiones/<empresa>` (solo lectura para el cliente);
- le envía un correo de SharePoint con el enlace a la plataforma (`…?e=<empresa>`);
- el cliente entra con **ese mismo correo**; si no es de Microsoft, recibe un código por correo.

Para agregar a otra persona de la misma empresa: **Invitar cliente → Dar acceso a otra persona**.

## Dónde quedan los datos

| Carpeta en SharePoint | Contenido | Quién edita |
|---|---|---|
| `Clientes/<empresa>/empresa.json` | Perfil, respuestas, plan, mensajes del cliente | Cliente |
| `Clientes/<empresa>/evidencias/` | Archivos de evidencia (cualquier formato) | Cliente |
| `Revisiones/<empresa>/revision.json` | Validaciones, comentarios, bitácora | Consultores |
| `Publico/benchmarks.json` | Promedios anónimos por sector | Consultores |

Puedes abrir las evidencias de cada cliente directamente en SharePoint.

## Problemas frecuentes

- **"No se pudo conectar con SharePoint"**: revisa `SP_HOST`, `SP_SITE_PATH` y que el consentimiento de administrador esté concedido.
- **El cliente ve "No tienes acceso"**: debe iniciar sesión con el mismo correo al que llegó la invitación.
- **Error 403 al invitar**: el sitio no permite compartir con externos (Paso 1.4).
- **"AADSTS50011 redirect URI"**: la dirección en Entra (Paso 2.3) debe ser idéntica a la de GitHub Pages, con diagonal final.

---
© Aztlan. Metodología, ponderaciones, matriz de materialidad, guías y Matriz Semáforo son propiedad de Aztlan.
Incluye `msal-browser.min.js` (Microsoft Authentication Library, licencia MIT).
