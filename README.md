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

## 3. Acceso al área interna

`acceso.html` pide usuario y contraseña. Las credenciales iniciales son:

```
Usuario:     equipo
Contraseña:  RedConecta2027
```

**Cámbielas antes de compartirlas con alguien.** Abra `herramientas/generar-acceso.html` en el
navegador, escriba el usuario y la contraseña nuevos, presione *Generar el bloque* y reemplace en
`acceso.html` las líneas que van de `const BOVEDA = {` hasta `};`. Desde ahí se pueden cambiar
también los nombres y las direcciones de las herramientas.

La contraseña no queda escrita en ninguna parte del sitio. Lo que viaja en el archivo es el listado
de herramientas cifrado con AES-GCM, con la clave derivada de usuario y contraseña mediante PBKDF2
(200.000 iteraciones). Sin la contraseña correcta no se pueden leer las direcciones, ni siquiera
mirando el código fuente de la página. Si pierde la contraseña, las direcciones no se recuperan:
hay que volver a generar el bloque con la herramienta.

La sesión dura mientras la pestaña esté abierta. Al cerrarla, vuelve a pedir la contraseña.

### Hasta dónde llega esta protección

Sirve para que el área interna no sea pública y no aparezca en buscadores. No es una barrera contra
alguien con conocimientos técnicos y una copia de la contraseña, porque todo el descifrado ocurre en
el navegador. Y, sobre todo, **no protege las aplicaciones en sí**: quien conozca la dirección
directa de la app de entrevistas entra sin pasar por aquí.

Para cerrar eso hacen falta dos cosas, que siguen pendientes:

1. **Que cada aplicación verifique la sesión al abrirse**, en lugar de mostrar la interfaz a
   cualquiera que llegue con la dirección.
2. **Row Level Security activada en Supabase**, para que las tablas con datos de estudiantes exijan
   usuario autenticado. Sin esto, cualquiera con la clave `anon` puede leer la tabla completa desde
   la consola del navegador.

Cuando quiera dar ese paso, Supabase Auth con una cuenta por persona reemplaza este mecanismo y
resuelve las dos cosas a la vez.

## 4. Fotografías y logotipos

Los espacios de imagen ya están montados y funcionan solos: mientras el archivo no exista, se ve un
marco gris con el nombre y la medida que se necesita; en cuanto usted deje la foto con ese nombre
exacto en la carpeta, el marco desaparece y la imagen ocupa su lugar. No hay que tocar el código.

Las imágenes ya están montadas. Para cambiar cualquiera, deje el archivo nuevo con el mismo nombre.

| Archivo | Medida | Dónde aparece |
|---|---|---|
| `assets/fotos/apertura.jpg` | 2400 × 1029 | Banda ancha bajo la portada |
| `assets/fotos/proceso.jpg` | 2200 × 943 | Banda ancha antes de "Alianzas" |
| `assets/fotos/equipo.jpg` | 1200 × 1600 | Vertical, junto a "Quiénes somos" |
| `assets/fotos/galeria-1..3.jpg` | 1400 × 933 | Sección "El programa" |
| `assets/fotos/compartir.jpg` | 1200 × 630 | Vista previa al compartir el enlace |
| `assets/logos/politecnico-grancolombiano.png` | 640 px | Franja de aliados |
| `assets/logos/ie-pablo-neruda.png` | 640 px | Franja de aliados |
| `assets/logos/griky.png` | 640 px | Franja de aliados |
| `assets/logos/fundacion-fernando-murillo.png` | 640 px | Franja de aliados |

Todas las imágenes provienen de originales a resolución completa, así que se ven nítidas en
cualquier pantalla. Las bandas anchas se comprimieron a calidad media para que la página cargue
rápido en conexiones móviles.

**Antes de publicar fotos con estudiantes.** El acuerdo de vinculación incluye una casilla de
autorización de uso de imagen. Publique únicamente fotos de quienes la hayan marcado y, tratándose
de menores de edad, con la firma del acudiente. Ante la duda, use encuadres donde no se identifiquen
rostros: manos trabajando, la sala del taller, materiales sobre la mesa, un grupo de espaldas. Nunca
acompañe una foto con el nombre completo de un menor.

Un espacio que no vaya a llenarse debe borrarse del HTML, no dejarse con el marcador visible.

## 5. Sistema visual

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

## 6. Mantenimiento

Todo el contenido está en HTML plano, con el texto legible dentro del archivo. Para cambiar una
sección, edite el párrafo directamente. Las ocho etapas del proceso viven en el arreglo `ETAPAS`
al final de `index.html`: cambiar el texto ahí actualiza la tira de escalones y la versión móvil
a la vez. Cada etapa tiene un nombre completo (`t`), que aparece en el detalle, y uno corto
(`corto`), que aparece en la tira.

Las variables de color y tipografía están declaradas al inicio del `<style>` de cada archivo.
