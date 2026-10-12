# Proyectos TGE

Hub de landings y catálogo de proyectos del Team Germanescarfullery. La versión oficial (con base de datos compartida) vive como Artifact en Claude; este repositorio guarda el código, una copia de los datos y las instrucciones para conectar el CRM.

Team Germanescarfullery · RE/MAX Metropolitana · exportado el 9 de octubre de 2026 · 29 proyectos

## Qué hay en este repositorio
- `Proyectos-TGE-completo.html`: la página entera en un solo archivo, con los 29 proyectos y sus fotos. Se abre con doble clic. Lo que se cambie ahí se guarda solo en ese navegador.
- `pagina/index.html`: el código completo de la página "Proyectos TGE" (hub de landings, catálogo para clientes, envío por WhatsApp, carruseles de Instagram, filtros por tipo, región y subregión, presupuesto, m²/ft², favoritos y bloqueo de proyectos duplicados).
- `datos.json`: los 29 proyectos (con tipologías, precios, amenidades, plan de pago, FAQ, etc.) y los asesores (sin contraseñas).
- `fotos/`: las fotos de los proyectos (hasta 3 por proyecto; la primera es la portada), comprimidas para que el ZIP quepa en un correo. En `datos.json`, cada proyecto tiene `fotos_archivos` con el nombre de cada foto. Los que tienen `fotos_incompletas: true` tenían más fotos en la página original.
- Pendiente de revisar: hay proyectos que parecen repetidos (Duo Terra, Palma Alta, Palmas de la Romana español/inglés, Green Garden IV) y uno con nombre de archivo ("Combinar 24 de julio de 2026 (4)"). 11 proyectos no tienen precio real o tienen precio de ejemplo (`precioRef: true`).

## Instrucciones para Claude (pegar esto en la otra cuenta)
1. Publica `pagina/index.html` como un Artifact con estas capacidades: `sample`, `assets`, `db`, `downloads`, `user`.
   - Base de datos: colecciones `proyectos`, `captadores` (asesores) y `envios`, más el documento `config/crm` con los campos `webhook` y `base`.
2. Sube cada foto de `fotos/` (los nombres de `fotos_archivos`) al almacén de assets del artifact. Luego carga cada proyecto de `datos.json` en la colección `proyectos`:
   - en el campo `fotos`, pon los nuevos ids de los assets, en el mismo orden;
   - borra los campos `fotos_archivos` e `id`.
3. Carga los asesores en `captadores`. Sus contraseñas se ponen de nuevo desde la página; la predeterminada es 1234.
4. **Conexión con el CRM** (https://crm.equipogerman.com): pega la URL de entrada del CRM en la página, en **Configuración → Webhook del CRM**. La página envía un POST con un JSON en `text/plain`:
   - Formulario de una landing:
     `{origen:"landing", evento:"lead", proyecto, proyectoId, asesor, asesorWhatsApp, captador, nombre, correo, whatsapp, mensaje, fecha, pagina}`
   - Favoritos del catálogo:
     `{origen:"catalogo", evento:"favoritos", cliente, usuario, asesor, favoritos:[{id, nombre}], fecha}`
   - Exportación de proyectos: botón en Configuración, genera `proyectos-tge.json`.
   - El CRM tiene que aceptar ese POST. La página lo envía sin CORS (`mode: no-cors`), así que no lee la respuesta.
