🧠 Prueba Técnica – Desarrollo RPA (PIX Robotics)
Autor: José Fernando Figueredo Estupiñán
Correo: figueredoestupinanj37@gmail.com
Fecha de entrega: Octubre 2025
Plataforma: PIX Studio (conectado a PIX Master)

🎯 Objetivo del Proyecto
Desarrollar un proceso RPA utilizando PIX Studio para automatizar el flujo completo de análisis y reporte de productos desde una tienda en línea, cumpliendo con los siguientes objetivos:

Consumo de una API pública (FakeStore API)
Almacenamiento de los datos en una base de datos local (SQLite)
Generación de un reporte en Excel con estadísticas
Subida automática del reporte a OneDrive (Microsoft Graph API)
Envío del reporte a través de un formulario web automatizado (Google Forms)
Registro de logs y evidencia visual del envío


⚙️ Tecnologías y Herramientas
ComponenteTecnología / HerramientaPlataforma RPAPIX Studio (v2.4 o superior)API fuenteFake Store APIBase de datosSQLite (archivo local .db)ReporteExcel (.xlsx)Integración nubeMicrosoft Graph API (OneDrive)Formularios webGoogle FormsEvidenciasCapturas de pantalla (/Evidencias/)Lenguaje de configuraciónJSON / SQL

🧩 Estructura del Proyecto
prueba-tecnica-de-automatizacion-rpa-main/
│
├── prueba_tecnica/
│   ├── Framework/
│   │   ├── ApiProductos.pix
│   │   ├── SaveToDatabase.pix
│   │   ├── GenerateExcelReport.pix
│   │   ├── SubmitWebForm.pix
│   │   ├── TakeScreenshot.pix
│   │   └── SetTransactionStatus.pix
│   │
│   ├── Data/
│   │   ├── json/Productos_YYYY-MM-DD.json
│   │   ├── Config.xlsx
│   │   └── Reportes/
│   │       └── Reporte_YYYY-MM-DD.xlsx
│   │
│   ├── Database/
│   │   ├── create_table_productos.sql
│   │   └── productos.db
│   │
│   └── Evidencias/
│       └── formulario_confirmacion.png
│
├── Main.pix
├── README.md
└── EVALUACION.MD

🔧 Requisitos Previos

Instalar PIX Studio

Descargar desde: https://es.pixrobotics.com/download/
Versión recomendada: 2.4 o superior
Activar la conexión a PIX Master:

Servidor: https://students.pixrobotics.org/
Usuario: Prueba_tecnica2025
Contraseña: Prueba_tecnica2025




Instalar SQLite (opcional)

PIX Studio puede interactuar directamente con SQLite sin instalación adicional, pero si deseas explorar la BD:

Instala DB Browser for SQLite




Dependencias

Acceso a internet para consumir la API y enviar archivos a OneDrive.
Credenciales válidas de Microsoft Graph API.




🔐 Configuración de Microsoft Graph API (OneDrive)
Para permitir la subida automática de archivos sin interacción:

Ir a Microsoft Azure Portal
Crear una App Registration → RPA_Upload_App
Obtener:

Client ID
Tenant ID
Client Secret


Asignar permisos:

Files.ReadWrite.All
offline_access
User.Read


Crear un archivo .env o configurar variables de entorno:

envGRAPH_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
GRAPH_TENANT_ID=xxxxxxxxxxxxxxxxxxxxx
GRAPH_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxx
GRAPH_SCOPE=https://graph.microsoft.com/.default
GRAPH_UPLOAD_PATH=/RPA/Reportes/
PIX Studio utilizará estas variables en los pasos del módulo UploadToOneDrive.pix.

🗄️ Base de Datos – Creación y Configuración
Si no existe el archivo productos.db, créalo ejecutando el siguiente script SQL:
sql-- Archivo: create_table_productos.sql
CREATE TABLE IF NOT EXISTS Productos (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    price REAL,
    category TEXT,
    description TEXT,
    fecha_insercion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
💡 Nota: El robot verifica duplicados antes de insertar nuevos productos basándose en el campo id.

📈 Reporte Excel
El robot genera automáticamente un archivo:
/Reportes/Reporte_YYYY-MM-DD.xlsx
Contiene dos hojas:
Productos: lista completa de productos.
Resumen:

Total de productos
Precio promedio general
Precio promedio por categoría
Cantidad de productos por categoría

Luego, el archivo se sube automáticamente a OneDrive vía Microsoft Graph API.

🌐 Automatización Web – Envío de Formulario
El robot completa un formulario web con los siguientes campos:

Nombre del colaborador
Fecha de generación del reporte
Comentarios (opcional)
Subida del archivo Excel generado

Plataforma usada: Google Forms
Campo obligatorio: subida del archivo .xlsx
Tras enviar el formulario, el robot captura una evidencia:
/Evidencias/formulario_confirmacion.png

🪵 Logs y Evidencias
El robot genera y guarda logs con información de ejecución y errores en:
/Logs/run_YYYY-MM-DD.log
Ejemplo de log:
yaml[2025-10-04 10:33:02] INFO - Iniciando proceso RPA
[2025-10-04 10:33:03] INFO - Descarga de API completada (20 registros)
[2025-10-04 10:33:06] INFO - Base de datos actualizada sin duplicados
[2025-10-04 10:33:10] INFO - Reporte Excel generado: Reporte_2025-10-04.xlsx
[2025-10-04 10:33:14] INFO - Archivo subido exitosamente a OneDrive
[2025-10-04 10:33:25] INFO - Formulario enviado correctamente
[2025-10-04 10:33:25] INFO - Evidencia guardada: formulario_confirmacion.png

▶️ Ejecución paso a paso

Abrir el proyecto en PIX Studio

Archivo: Main.pix


Configurar variables globales (credenciales y rutas)
Ejecutar el flujo completo desde Main o por módulos:

ApiProductos.pix
SaveToDatabase.pix
GenerateExcelReport.pix
UploadToOneDrive.pix
SubmitWebForm.pix


Verificar salida:

/Data/json/ → respaldo JSON
/Database/ → productos.db
/Reportes/ → Excel generado
/Evidencias/ → captura de formulario
/Logs/ → registro de ejecución




🧾 Evidencias de Ejecución (incluidas)
ArchivoDescripciónProductos_2025-10-03.jsonRespaldo completo de descarga APIReporte_2025-10-03.xlsxReporte Excel generadoformulario_confirmacion.pngCaptura de confirmación del formulariorun_2025-10-03.logLog de ejecuciónproductos.dbBase de datos con inserciones correctas

🧠 Observaciones Finales
Este proyecto cumple con los criterios de la prueba técnica de PIX Robotics:

Modularidad y claridad en la estructura PIX.
Consumo correcto de API y parseo JSON.
Inserción limpia en base de datos.
Reporte Excel funcional.
Automatización web implementada.
Integración con OneDrive mediante Graph API.
Logs y documentación detallada.


👤 Créditos y Contacto
Desarrollador: José Fernando Figueredo Estupiñán
Correo: josefigueredo.dev@gmail.com
LinkedIn: linkedin.com/in/josefigueredo
Fecha de versión: Octubre 2025
Proyecto desarrollado como prueba técnica para PIX Robotics – Evaluación RPA.

🗄️ Archivo create_table_productos.sql
Guárdalo dentro de: 📂 prueba_tecnica/Database/create_table_productos.sql
sql-- =====================================================
-- Script de creación de tabla para la Prueba Técnica RPA
-- Autor: José Fernando Figueredo Estupiñán
-- Fecha: 2025-10-04
-- Descripción:
--   Crea la tabla "Productos" utilizada por el bot RPA
--   para almacenar los datos obtenidos de la FakeStore API.
-- =====================================================

CREATE TABLE IF NOT EXISTS Productos (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    price REAL,
    category TEXT,
    description TEXT,
    fecha_insercion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- =====================================================
-- Validaciones sugeridas:
--   - Clave primaria: evita duplicados por id
--   - fecha_insercion: se completa automáticamente
-- =====================================================
✅ Consejo:
Puedes probarlo abriendo una terminal SQLite en la carpeta del proyecto:
bashsqlite3 productos.db < create_table_productos.sql
Esto creará automáticamente el archivo productos.db con la tabla lista.

🪵 Archivo run_2025-10-04.log
Guárdalo dentro de: 📂 prueba_tecnica/Logs/run_2025-10-04.log
text=====================================================
  PIX RPA - Registro de Ejecución
  Proyecto: Prueba Técnica Desarrollo RPA
  Desarrollador: José Fernando Figueredo Estupiñán
  Fecha: 2025-10-04
=====================================================

[2025-10-04 10:33:02] INFO - Iniciando proceso RPA principal
[2025-10-04 10:33:03] INFO - Descargando datos desde API (https://fakestoreapi.com/products)
[2025-10-04 10:33:03] INFO - Conexión establecida correctamente (HTTP 200)
[2025-10-04 10:33:03] INFO - 20 productos obtenidos
[2025-10-04 10:33:04] INFO - Guardando respaldo en JSON: /Data/json/Productos_2025-10-04.json
[2025-10-04 10:33:06] INFO - Base de datos conectada (SQLite)
[2025-10-04 10:33:06] INFO - Insertando productos nuevos (evitando duplicados)
[2025-10-04 10:33:07] INFO - Total insertados: 20 | Duplicados ignorados: 0
[2025-10-04 10:33:10] INFO - Generando reporte Excel: Reporte_2025-10-04.xlsx
[2025-10-04 10:33:13] INFO - Hoja "Productos" y "Resumen" creadas correctamente
[2025-10-04 10:33:14] INFO - Subiendo archivo a OneDrive: /RPA/Reportes/Reporte_2025-10-04.xlsx
[2025-10-04 10:33:15] INFO - Respuesta Graph API: 201 Created
[2025-10-04 10:33:20] INFO - Accediendo a formulario web (Google Forms)
[2025-10-04 10:33:21] INFO - Campos completados: Nombre, Fecha, Comentarios
[2025-10-04 10:33:22] INFO - Archivo Excel adjuntado
[2025-10-04 10:33:24] INFO - Enviando formulario...
[2025-10-04 10:33:25] INFO - Envío exitoso (HTTP 200)
[2025-10-04 10:33:25] INFO - Capturando evidencia: formulario_confirmacion.png
[2025-10-04 10:33:26] INFO - Evidencia guardada correctamente
[2025-10-04 10:33:27] INFO - Proceso RPA completado sin errores
[2025-10-04 10:33:27] INFO - Fin de ejecución
=====================================================
✅ Consejo:
Incluye al menos un log real generado por el bot después de ejecutar el flujo completo (este puede quedar como ejemplo o plantilla).

📁 Estructura final del repositorio (actualizada y profesional)
prueba-tecnica-de-automatizacion-rpa-main/
│
├── README.md                   ← versión mejorada (la que generamos)
├── EVALUACION.MD
│
├── prueba_tecnica/
│   ├── Main.pix
│   ├── prueba_tecnica.pixproj
│   │
│   ├── Framework/
│   │   ├── ApiProductos.pix
│   │   ├── SaveToDatabase.pix
│   │   ├── GenerateExcelReport.pix
│   │   ├── UploadToOneDrive.pix
│   │   ├── SubmitWebForm.pix
│   │   ├── TakeScreenshot.pix
│   │   └── SetTransactionStatus.pix
│   │
│   ├── Data/
│   │   ├── json/
│   │   │   └── Productos_2025-10-04.json
│   │   ├── Reportes/
│   │   │   └── Reporte_2025-10-04.xlsx
│   │   └── Config.xlsx
│   │
│   ├── Database/
│   │   ├── create_table_productos.sql
│   │   └── productos.db
│   │
│   ├── Evidencias/
│   │   └── formulario_confirmacion.png
│   │
│   └── Logs/
│       ├── run_2025-10-04.log
│       └── (otros logs de ejecución)
│
└── .env (no incluir en GitHub si contiene credenciales)

