# Sitio web — Fundación Red Conecta

Sitio institucional estático más un área de acceso restringido a las herramientas internas.
No requiere servidor propio, ni base de datos para la parte pública, ni proceso de compilación.

```
index.html      Sitio público
acceso.html     Login y portal de herramientas internas
assets/         Logotipos y favicon (fondo transparente)
vercel.json     Cabeceras de seguridad y caché
robots.txt      Excluye /acceso de los buscadores
```

---

## 1. Datos de contacto

Busque los corchetes en `index.html` y reemplácelos:

Los datos de contacto ya están puestos: fundacionredconecta@gmail.com, +57 313 401 1695 y
Bogotá, Colombia. Aparecen en la portada, en "Protección de datos", en "Hablemos" y en la
página de acceso.

Los datos de identificación de la entidad (NIT, representante legal, domicilio fiscal) no aparecen
en el sitio. Cuando decida publicarlos, el lugar natural es la sección "Protección de datos" o el
pie de página.

En la sección **Alianzas** hay un párrafo sobre aliados. Solo publique el nombre y el logo de una
institución cuando ella lo haya autorizado por escrito; mientras tanto, deje el texto actual.

---

## 2. Publicar en Vercel

1. Cree un repositorio en GitHub, por ejemplo `sitio-red-conecta`, y suba estos archivos a la raíz.
2. En Vercel: **Add New → Project → Import Git Repository**.
3. Framework Preset: **Other**. No hay build command ni output directory.
4. **Deploy**.

Cada vez que haga un commit en GitHub, Vercel vuelve a publicar solo.

### Dominio propio

Cómprelo donde prefiera (`.org` o `.org.co` son los habituales para una fundación) y en Vercel vaya a
**Settings → Domains → Add**. Vercel le indica los registros DNS que debe crear en su proveedor y
emite el certificado HTTPS automáticamente.

---

## 3. Configurar el acceso al área interna

El login usa **Supabase Auth**, el mismo proyecto que ya tenía previsto para sincronizar las entrevistas.

1. En [supabase.com](https://supabase.com) cree un proyecto (plan gratuito).
2. **Authentication → Providers → Email**: deje activado el correo y **desactive** el registro
   público (*Allow new users to sign up*). Los usuarios se crean a mano.
3. **Authentication → Users → Add user**: cree una cuenta por cada persona autorizada.
4. **Project Settings → API**: copie `Project URL` y la clave `anon public`.
5. Abra `acceso.html` y complete el bloque de configuración al inicio del `<script>`:

```js
const SUPABASE_URL      = "https://xxxxxxxx.supabase.co";
const SUPABASE_ANON_KEY = "eyJhbGciOi...";

const APLICACIONES = [
  { nombre: "Entrevistas familiares", descripcion: "...", url: "https://entrevistas-red-conecta.vercel.app" },
  ...
];
```

La clave `anon public` está diseñada para ir en el navegador; no es un secreto. Lo que protege los
datos son las políticas de la base, no ocultar la clave.

---

## 4. Punto crítico de seguridad

**El portal solo muestra enlaces. No protege las aplicaciones.**

Si alguien conoce la dirección directa de la aplicación de entrevistas, entra sin pasar por el login.
Para cerrar ese hueco hacen falta dos cosas:

1. **Cada aplicación debe verificar la sesión al abrirse.** Al inicio de cada app, consultar
   `supabase.auth.getSession()` y, si no hay sesión, redirigir al login en lugar de mostrar la
   interfaz. Es el mismo bloque de código en las tres.
2. **Row Level Security activada en Supabase.** Cada tabla con datos de estudiantes debe tener RLS
   encendida y una política que exija usuario autenticado. Sin esto, cualquiera con la clave `anon`
   puede leer la tabla completa desde la consola del navegador.

La sesión se guarda por dominio. Si las aplicaciones viven en dominios distintos
(`entrevistas-xxx.vercel.app` y `redconecta.org`), cada una pedirá login por separado. Para una sola
sesión compartida, publíquelas como subdominios del dominio propio: `entrevistas.redconecta.org`,
`vocacional.redconecta.org`.

---

## 5. Fotografías y logotipos

Los espacios de imagen ya están montados y funcionan solos: mientras el archivo no exista, se ve un
marco gris con el nombre y la medida que se necesita; en cuanto usted deje la foto con ese nombre
exacto en la carpeta, el marco desaparece y la imagen ocupa su lugar. No hay que tocar el código.

Las imágenes ya están montadas. Para cambiar cualquiera, deje el archivo nuevo con el mismo nombre.

| Archivo | Medida | Dónde aparece |
|---|---|---|
| `assets/fotos/apertura.jpg` | 1800 × 771 | Banda ancha bajo la portada |
| `assets/fotos/proceso.jpg` | 1800 × 771 | Banda ancha antes de "Alianzas" |
| `assets/fotos/equipo.jpg` | 900 × 1200 | Vertical, junto a "Quiénes somos" |
| `assets/fotos/galeria-1..3.jpg` | 1000 × 667 | Sección "El programa" |
| `assets/fotos/compartir.jpg` | 1200 × 630 | Vista previa al compartir el enlace |
| `assets/logos/politecnico-grancolombiano.png` | 640 px | Franja de aliados |
| `assets/logos/ie-pablo-neruda.png` | 640 px | Franja de aliados |
| `assets/logos/griky.png` | 640 px | Franja de aliados |
| `assets/logos/fundacion-fernando-murillo.png` | 640 px | Franja de aliados |

**Resolución.** Las fotos entregadas venían a unos 500 px de ancho, el tamaño de vista previa de los
bancos de imágenes. Para las bandas anchas hubo que ampliarlas, así que en pantalla grande se ven
algo suaves. Con los originales a resolución completa la nitidez mejora de inmediato.

**Antes de publicar fotos con estudiantes.** El acuerdo de vinculación incluye una casilla de
autorización de uso de imagen. Publique únicamente fotos de quienes la hayan marcado y, tratándose
de menores de edad, con la firma del acudiente. Ante la duda, use encuadres donde no se identifiquen
rostros: manos trabajando, la sala del taller, materiales sobre la mesa, un grupo de espaldas. Nunca
acompañe una foto con el nombre completo de un menor.

Un espacio que no vaya a llenarse debe borrarse del HTML, no dejarse con el marcador visible.

## 6. Sistema visual

La dirección de diseño es editorial y tipográfica: retícula de doce columnas visible mediante reglas
de un pixel, tipografía de gran escala como elemento principal, y color usado como acontecimiento
puntual en vez de decoración distribuida.

- **Azul Medianoche `#003366`** sostiene la portada, la declaración, el contacto y el pie.
- **Turquesa `#00CED1`** aparece en tres lugares y nada más: la regla que atraviesa el titular
  (el mismo recurso que el logotipo usa entre "Fundación" y "Red Conecta"), el nodo que marca listas
  y enlaces de acceso, y el bloque completo del Círculo de Contribución.
- **Lora** carga el peso expresivo en tamaños grandes; **Montserrat** queda para rótulos, cifras,
  navegación y microtexto.
- Sin esquinas redondeadas, sin sombras, sin tarjetas. La jerarquía la construyen las reglas,
  la escala y el espacio.
- Una sola animación en toda la página: la entrada del titular y el trazo de la regla al cargar.
  Se desactiva sola si el sistema operativo pide movimiento reducido.

## 7. Mantenimiento

Todo el contenido está en HTML plano, con el texto legible dentro del archivo. Para cambiar una
sección, edite el párrafo directamente. Las ocho etapas del proceso viven en el arreglo `ETAPAS`
al final de `index.html`: cambiar el texto ahí actualiza la tira de escalones y la versión móvil
a la vez. Cada etapa tiene un nombre completo (`t`), que aparece en el detalle, y uno corto
(`corto`), que aparece en la tira.

Las variables de color y tipografía están declaradas al inicio del `<style>` de cada archivo.
