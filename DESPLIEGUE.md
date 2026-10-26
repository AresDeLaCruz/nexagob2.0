# Guía de despliegue - NexaGob

## Estructura
- **Frontend** → Vercel
- **Backend** → Render
- **Base de datos** → Aiven (MySQL)

---

## 1. Base de datos (Aiven)

1. Entra a https://aiven.io y crea cuenta gratis
2. Crea un servicio **MySQL**
3. Anota:
   - Host
   - Port
   - User (normalmente `avnadmin`)
   - Password
4. Crea la base de datos `nexagob` (o usa la que te den)
5. Importa tu esquema (tablas) desde phpMyAdmin o con el SQL de tu XAMPP

---

## 2. Backend en Render

1. Sube el proyecto a **GitHub** (solo la carpeta `server` o el repo completo)
2. Entra a https://render.com → New → Web Service
3. Conecta el repositorio
4. Configuración:
   - **Root Directory**: `server`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Instance Type**: Free
5. En **Environment** agrega estas variables:

```
DB_HOST=tu-host.aivencloud.com
DB_USER=avnadmin
DB_PASSWORD=tu_password
DB_NAME=nexagob
DB_PORT=12345
DB_SSL=true
EMAIL_USER=nexagob@gmail.com
EMAIL_PASS=tu_app_password_de_gmail
GOOGLE_CLIENT_ID=275398566455-t55t4nleuplm6pvuj5cbape9ut483jdb.apps.googleusercontent.com
FRONTEND_URL=https://tu-app.vercel.app
```

6. Despliega. Copia la URL que te da Render (ej: `https://nexagob-api.onrender.com`)

---

## 3. Frontend en Vercel

1. Entra a https://vercel.com → New Project
2. Importa el mismo repositorio de GitHub
3. Configuración:
   - **Root Directory**: deja la raíz del proyecto (donde está `package.json` del frontend)
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
4. En **Environment Variables** agrega:

```
VITE_API_URL=https://tu-backend.onrender.com
```

(usa la URL exacta de Render, **sin** barra al final)

5. Despliega.

---

## 4. Actualizar CORS

Cuando tengas la URL de Vercel, ve a Render → Environment y actualiza:

```
FRONTEND_URL=https://tu-app.vercel.app
```

Luego haz **Manual Deploy** en Render.

---

## Notas importantes

- El plan gratis de Render **se duerme** después de 15 minutos sin tráfico. La primera petición puede tardar ~30-50 segundos.
- Nunca subas archivos `.env` a GitHub.
- La App Password de Gmail la generas en: Google Account → Seguridad → Verificación en 2 pasos → Contraseñas de aplicaciones.
