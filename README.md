# Balashte Control

Sistema web con frontend en React y backend en NestJS.

## Estructura del proyecto

```txt
Balashte-Control/
├── backend-nestjs/
└── orchid-frontend/
```

## Base de datos en Supabase

El backend quedó preparado para conectarse a Supabase usando PostgreSQL.

Supabase usa PostgreSQL, por lo que si la base original estaba en MySQL primero debes migrar la estructura y datos a PostgreSQL. Puedes hacerlo desde la guía de Supabase para migrar MySQL a Supabase o usando pgloader.

### Variables del backend

En el servicio donde despliegues el backend, por ejemplo Render o Railway, configura estas variables:

```env
PORT=4000
HOST=0.0.0.0
DATABASE_URL=pega_aqui_la_connection_string_de_supabase
DB_SSL=true
JWT_SECRET=cambia_esta_clave_por_una_muy_larga
CORS_ORIGINS=http://localhost:5173,https://tu-sitio.netlify.app
```

La `DATABASE_URL` debe salir de Supabase en:

```txt
Project Settings > Database > Connection string
```

No subas tu archivo `.env` a GitHub.

## Backend NestJS

Entrar a la carpeta del backend:

```bash
cd backend-nestjs
npm install
npm run build
npm start
```

Para desarrollo:

```bash
npm run dev
```

Endpoint de prueba:

```txt
/api o /health según el controlador que quieras probar
```

## Frontend React con Netlify

El frontend está en `orchid-frontend` y usa Vite.

En Netlify configura:

```txt
Base directory: orchid-frontend
Build command: npm run build
Publish directory: dist
```

También se agregó `netlify.toml` en la raíz del proyecto. Antes de desplegar, cambia esta parte por la URL real de tu backend:

```toml
to = "https://TU-BACKEND.onrender.com/api/:splat"
```

por ejemplo:

```toml
to = "https://balashte-control-api.onrender.com/api/:splat"
```

## Variable opcional del frontend

El frontend puede usar `VITE_API_URL` si prefieres no usar el proxy de Netlify:

```env
VITE_API_URL=https://tu-backend.onrender.com/api
```

Si no defines esa variable, el frontend usará `/api` y Netlify lo redirigirá al backend usando `netlify.toml`.

## Orden recomendado de despliegue

1. Migrar la base de datos de MySQL a Supabase PostgreSQL.
2. Desplegar el backend NestJS en Render, Railway o un VPS.
3. Copiar la URL del backend.
4. Actualizar `netlify.toml` con la URL real del backend.
5. Desplegar el frontend en Netlify.
6. Agregar la URL de Netlify en `CORS_ORIGINS` del backend.
