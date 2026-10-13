# Proyectos TGE

Hub de landings y catálogo de proyectos del Team Germanescarfullery. La versión oficial (con base de datos compartida) vive como Artifact en Claude; este repositorio guarda el código, una copia de los datos y las instrucciones para conectar el CRM.

Team Germanescarfullery · RE/MAX Metropolitana · actualizado el 13 de octubre de 2026 · 47 proyectos

## Qué hay en este repositorio
- `Proyectos-TGE-completo.html`: la página entera en un solo archivo, con los 47 proyectos y sus fotos. Se abre con doble clic. Lo que se cambie ahí se guarda solo en ese navegador.
- `pagina/index.html`: el código completo de la página "Proyectos TGE" (hub de landings, catálogo para clientes, envío por WhatsApp, carruseles de Instagram, filtros por tipo, región y subregión, presupuesto, m²/ft², favoritos y bloqueo de proyectos duplicados).
- `Proyectos-que-faltan.csv`: los 92 proyectos del CRM (de 129) que todavía no están en la página, con precios, entrega y contacto.
- `datos.json`: los 47 proyectos (con tipologías, precios, amenidades, plan de pago, FAQ, etc.) y los asesores (sin contraseñas).
- `fotos/`: las fotos de los proyectos (hasta 3 por proyecto; la primera es la portada), comprimidas para que el ZIP quepa en un correo. En `datos.json`, cada proyecto tiene `fotos_archivos` con el nombre de cada foto. Los que tienen `fotos_incompletas: true` tenían más fotos en la página original.
- Pendiente de revisar: 7 proyectos están "Por completar" (sin precio o sin fotos). Posibles repetidos por confirmar: Palmeras Resipal / Independent Villas; Residencias Meliá / La Vista by Meliá; los planos "Tipología Arena", "Tipología Mar" y "Residencial Ciudad del Sol" (Verón).

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
