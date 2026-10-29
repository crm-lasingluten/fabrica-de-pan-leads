# Muestras Pan Proteico · Fábrica de Pan

Página web para llevar el control de las muestras de Pan Proteico enviadas a nutricionistas: a quién se le envió, quién contestó y a quién le interesa el producto.

- **Página:** se publica gratis en GitHub Pages.
- **Datos:** se guardan en Supabase (plan gratuito).
- **Acceso:** solo con email y contraseña. Los usuarios los crea el administrador.

Tiempo estimado de instalación: 20 a 30 minutos.

---

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `FABRICA-DE-PAN_supabase.sql` | Crea la tabla de pedidos y su seguridad en Supabase. |
| `FABRICA-DE-PAN_index.html` | La página. **Al subirla a GitHub hay que renombrarla a `index.html`.** |
| `FABRICA-DE-PAN_README.md` | Esta guía. Se puede subir al repositorio como `README.md`. |

---

## Paso 1 · Crear el proyecto en Supabase

1. Entrar a [supabase.com](https://supabase.com) y crear una cuenta, idealmente con un email de la empresa.
2. **New project**:
   - **Name:** `muestras-fabrica-de-pan`
   - **Database password:** generar una y guardarla en un lugar seguro.
   - **Region:** *South America (São Paulo)*, la más cercana a Argentina.
3. Esperar uno o dos minutos a que el proyecto termine de crearse.

## Paso 2 · Crear la tabla

1. En el menú izquierdo, abrir **SQL Editor** y después **New query**.
2. Abrir `FABRICA-DE-PAN_supabase.sql`, copiar todo el contenido y pegarlo.
3. Tocar **Run**. Tiene que aparecer *Success. No rows returned*.

El script crea la tabla `muestras`, deja que solo los usuarios con sesión iniciada lean y editen, y activa el tiempo real para que dos personas vean los cambios del otro sin recargar. Ejecutarlo dos veces no rompe nada.

## Paso 3 · Cerrar el registro público y crear los usuarios

1. Ir a **Authentication** y abrir la configuración de **Sign In / Providers**. Desactivar **Allow new users to sign up**. Así nadie puede crearse una cuenta por su cuenta.
2. Ir a **Authentication**, después **Users** y **Add user**, y elegir **Create new user**:
   - Email y contraseña de cada persona que va a usar la lista.
   - Marcar **Auto Confirm User**.
3. Repetir el paso 2 para cada usuario. Para quitarle el acceso a alguien, se borra su usuario de esta misma pantalla.

## Paso 4 · Copiar los datos de conexión

1. Ir a **Project Settings** y abrir **API** (en algunas versiones se llama **Data API** o **API Keys**).
2. Copiar dos datos:
   - **Project URL**, por ejemplo `https://abcdefgh.supabase.co`
   - La clave **anon public**, que en versiones nuevas se llama **publishable key**.
3. Abrir `FABRICA-DE-PAN_index.html` con un editor de texto (Bloc de notas, TextEdit o VS Code) y buscar la sección **CONFIGURACIÓN**, al principio del archivo:

   ```js
   const SUPABASE_URL = "https://TU-PROYECTO.supabase.co";
   const SUPABASE_KEY = "TU-CLAVE-PUBLICA-ANON";
   ```

   Reemplazar los dos valores, dejando las comillas, y guardar.

> ⚠️ **Nunca pegar la clave `service_role` ni la `secret key`.** Esa clave salta toda la seguridad y la página es pública. La clave anon es segura de publicar: sin iniciar sesión no da acceso a ningún dato.

## Paso 5 · Publicar en GitHub Pages

1. Entrar a [github.com](https://github.com) y crear una cuenta si hace falta.
2. **New repository**:
   - **Name:** `muestras-fabrica-de-pan`
   - **Public.** GitHub Pages gratuito necesita un repositorio público. Los datos no quedan expuestos: están en Supabase, protegidos por la contraseña.
3. En el repositorio nuevo, tocar **Add file** y después **Upload files**. Subir `index.html`, es decir, el `FABRICA-DE-PAN_index.html` ya renombrado y con los datos del paso 4. Tocar **Commit changes**.
4. Ir a **Settings**, abrir **Pages** y en **Source** elegir **Deploy from a branch**. Después elegir **Branch: `main`**, carpeta **`/ (root)`**, y tocar **Save**.
5. En uno o dos minutos aparece la dirección de la página, por ejemplo `https://usuario.github.io/muestras-fabrica-de-pan/`.

## Paso 6 · Configurar la recuperación de contraseña

Este paso hace falta para que funcione el enlace "Olvidé mi contraseña".

1. En Supabase, ir a **Authentication** y abrir **URL Configuration**.
2. En **Site URL**, pegar la dirección de GitHub Pages del paso 5.
3. En **Redirect URLs**, agregar la misma dirección y guardar.

## Paso 7 · Probar

1. Abrir la dirección de la página e ingresar con uno de los usuarios del paso 3.
2. Cargar un pedido de prueba, cambiar sus estados y después eliminarlo.

---

## Cómo se usa

- **Nuevo pedido:** cargar los datos de la nutricionista cuando pide muestras.
- **Estados:** se cambian tocándolos directo en la lista.
  - **Muestra:** No → Enviada. Al marcarla como enviada, se guarda la fecha del día.
  - **Contestó:** No → Sí.
  - **Le interesa:** Sin definir → Sí → No.
- **Lápiz:** edita todos los datos y las notas, o elimina el pedido.
- **Contadores de arriba:** funcionan también como filtros. Por ejemplo, "Por enviar" muestra solo las muestras pendientes.
- **Barra amarilla a la izquierda:** marca muestras enviadas hace más de 10 días que no tuvieron respuesta. Conviene hacerles seguimiento.
- **Exportar CSV:** descarga la lista para abrirla en Excel.

## Problemas frecuentes

| Qué pasa | Qué hacer |
|---|---|
| La página dice "Falta configurar" | No se completaron `SUPABASE_URL` y `SUPABASE_KEY` (paso 4), o se subió el archivo sin guardar los cambios. |
| "Email o contraseña incorrectos" | Revisar el usuario en Supabase, en Authentication > Users. Desde ahí se puede cambiar la contraseña. |
| Ingresa pero no ve pedidos ni puede guardar | Volver a ejecutar el SQL del paso 2. |
| El email de recuperación lleva a una página que no existe | Revisar el paso 6. |
| La página no carga los cambios recién subidos | GitHub Pages tarda unos minutos. Recargar con Ctrl + F5. |

## Datos personales

La lista guarda nombre, matrícula, teléfono, email y dirección de profesionales. Conviene:

- Dar acceso solo a quien lo necesita.
- Borrar los usuarios de quienes dejen de trabajar con la lista.
- Usar los datos solo para el seguimiento de las muestras.

En Argentina rige la Ley 25.326 de Protección de Datos Personales.

**Plan gratuito de Supabase:** si el proyecto pasa una semana sin uso, Supabase lo pausa. Se reactiva desde el panel con **Restore project** y los datos no se pierden.
