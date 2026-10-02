# Sitio web — Fundación Red Conecta

https://fundacionredconecta.org · sitio estático en Vercel, publicado desde GitHub.

```
index.html        Inicio (español)
alianzas.html     Colegios, universidades y servicios complementarios
programa.html     Metodología y Círculo de Contribución
apoyar.html       Apadrinamiento y donaciones
en/               Las mismas cuatro páginas en inglés
acceso.html       Portal interno, protegido con contraseña
404.html          Página de error
assets/estilo.css Hoja de estilo compartida por todas las páginas
assets/           Logotipos, fotografías y logos de aliados
herramientas/     Generadores de contraseña (no enlazados desde el sitio)
sitemap.xml       Las ocho páginas, con sus equivalencias de idioma
robots.txt        Excluye /acceso y /herramientas
vercel.json       Cabeceras y direcciones limpias
```

**Todo el diseño vive en `assets/estilo.css`.** Para cambiar un color, una tipografía o un espaciado,
se edita ahí una sola vez y cambia en las diez páginas. Nunca vuelva a pegar estilos dentro de un HTML.

Las direcciones no llevan `.html` porque Vercel tiene activado `cleanUrls`. Los enlaces internos
apuntan a `/alianzas`, `/en/programa`, etc.

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

