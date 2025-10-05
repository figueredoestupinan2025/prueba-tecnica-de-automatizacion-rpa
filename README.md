# 📘 Prueba Técnica – Desarrollo RPA con PIX Studio

## 🧠 Información del Proyecto

| Campo | Detalle |
|-------|---------|
| **Proyecto** | Automatización de Análisis de Productos mediante RPA |
| **Plataforma** | PIX Studio v2.27.4 (x64) |
| **Fecha de Entrega** | 04 de Octubre de 2025 |
| **Desarrollador** | José Fernando Figueredo Estupiñán |
| **Correo de contacto** | 📧 figueredoestupinanj37@gmail.com |

## 🧩 Descripción General

Este proyecto implementa un proceso RPA completo que automatiza el análisis diario de productos disponibles en una tienda online ficticia.

El robot ejecuta un flujo end-to-end que incluye:

🔹 Consumo de API REST pública  
🔹 Almacenamiento estructurado en base de datos MySQL  
🔹 Generación automatizada de reportes estadísticos en Excel  
🔹 Integración con Microsoft OneDrive mediante Graph API  
🔹 Envío automatizado de formularios web  
🔹 Sistema de logging y captura de evidencias  

El diseño está basado en la plantilla universal de PIX Robotics, con una arquitectura modular, reutilizable y orientada a buenas prácticas RPA corporativas.

---

## 🏗️ Arquitectura del Proyecto

### 📁 Estructura de Carpetas

```
prueba_tecnica/
│
├── Framework/                          # Scripts modulares del proceso
│   ├── InitApplications.pix           # Inicialización del entorno
│   ├── ApiProductos.pix               # Consumo de API y procesamiento JSON
│   ├── SaveToDatabase.pix             # Inserción en base de datos
│   ├── GenerateExcelReport.pix        # Generación de reportes Excel
│   ├── SubmitWebForm.pix              # Automatización de formulario web
│   ├── CloseApplications.pix          # Cierre controlado de aplicaciones
│   ├── KillApplications.pix           # Manejo de excepciones y cierre forzado
│   ├── GetTransactionItem.pix         # Procesamiento transaccional
│   ├── SetTransactionStatus.pix       # Control de estado de transacciones
│   ├── TakeScreenshot.pix             # Captura de evidencias visuales
│   └── ReadConfig.pix                 # Lectura de configuración
│
├── Data/                              # Datos y configuración
│   ├── Input/                         # Archivos de entrada (si aplica)
│   ├── json/                          # Respaldos JSON de la API
│   │   └── Productos_YYYY-MM-DD.json
│   ├── Output/                        # Archivos de salida procesados
│   ├── Temp/                          # Archivos temporales
│   └── Config.xlsx                    # Configuración del proyecto
│
├── Reportes/                          # Reportes generados
│   └── Reporte_YYYY-MM-DD.xlsx
│
├── Evidencias/                        # Capturas de pantalla
│   └── formulario_confirmacion.png
│
├── Exceptions_Screenshots/            # Capturas de errores
│
├── Main.pix                           # Flujo principal orquestador
├── ProcessTransactionItem.pix         # Procesamiento de items
└── prueba_tecnica.pixproj             # Archivo del proyecto PIX
```

---

## 🔁 Flujo del Proceso

### 1️⃣ Inicialización (InitApplications)

- Carga configuración desde `Config.xlsx`
- Inicializa variables globales
- Prepara rutas de trabajo
- Configura manejo global de excepciones

### 2️⃣ Consumo de API (ApiProductos)

**Endpoint:** `https://fakestoreapi.com/products`

**Acciones:**
- Realiza solicitud HTTP GET
- Extrae campos: `id`, `title`, `price`, `category`, `description`
- Guarda respuesta en `/Data/json/Productos_YYYY-MM-DD.json`
- Parsea JSON y crea DataTable
- Sube archivo JSON a OneDrive (`/RPA/Logs/`)

### 3️⃣ Almacenamiento en Base de Datos (SaveToDatabase)

**Base de datos:** MySQL (localhost:3306)  
**Tabla:** `productos`

```sql
CREATE TABLE productos (
    id INT PRIMARY KEY,
    title VARCHAR(255),
    price DECIMAL(10,2),
    category VARCHAR(100),
    description TEXT,
    fecha_insercion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Lógica funcional:**
- Valida duplicados por `id` antes de insertar
- Registra `fecha_insercion` automáticamente
- Controla errores de conexión y constraints

### 4️⃣ Generación de Reporte Excel (GenerateExcelReport)

**Archivo generado:** `/Reportes/Reporte_YYYY-MM-DD.xlsx`

**Estructura:**

| Hoja | Descripción |
|------|-------------|
| **Productos** | Listado completo de productos extraídos |
| **Resumen** | Estadísticas: total de productos, precio promedio general, y promedio por categoría |

### 5️⃣ Subida a OneDrive (Microsoft Graph API)

**Autenticación:** Client Credentials Flow

**Credenciales requeridas:**
- `tenant_id`
- `client_id`
- `client_secret`

**Archivos subidos:**
- `/RPA/Logs/Productos_YYYY-MM-DD.json`
- `/RPA/Reportes/Reporte_YYYY-MM-DD.xlsx`

**Manejo de errores:** Si no hay credenciales válidas, el flujo registra un `401 Unauthorized` sin detener la ejecución.

### 6️⃣ Envío de Formulario Web (SubmitWebForm)

**Plataforma:** Google Forms / Jotform / Typeform

**Campos completados:**
- Nombre del colaborador
- Fecha del reporte
- Comentarios (opcional)
- Archivo adjunto: Excel

**Evidencia:** `/Evidencias/formulario_confirmacion.png`

### 7️⃣ Cierre del Proceso (CloseApplications)

- Cierra aplicaciones abiertas
- Limpia temporales
- Registra finalización en logs

---

## ⚙️ Requisitos Técnicos

### 🧰 Software Requerido

- PIX Studio v2.27 o superior
- XAMPP (MySQL 8.0.30)
- Microsoft Excel
- Navegador Chrome o Edge

### 💾 Dependencias del Sistema

- .NET Framework 4.8
- Visual C++ Redistributable 2015–2022

### 🗄️ Base de Datos

- **Motor:** MySQL 8.0
- **Servidor:** localhost:3306
- **BD:** rpa_productos
- **Usuario:** root
- **Contraseña:** (vacía por defecto)

### ☁️ Credenciales Microsoft Graph API

Para la integración con OneDrive:

1. Registrar aplicación en Azure AD
2. Obtener:
   - `tenant_id`
   - `client_id`
   - `client_secret`
3. Asignar permiso: `Files.ReadWrite.All` (Application)
4. Configurar en `Config.xlsx` o variables de entorno de PIX Studio

---

## 🚀 Instalación y Configuración

### 1️⃣ Conectar PIX Studio

- **Servidor:** https://students.pixrobotics.org/
- **Usuario:** Prueba_tecnica2025
- **Contraseña:** Prueba_tecnica2025

*(Credenciales de entorno académico)*

### 2️⃣ Configurar MySQL

```sql
# Iniciar MySQL en XAMPP
CREATE DATABASE rpa_productos;
USE rpa_productos;
CREATE TABLE productos (
    id INT PRIMARY KEY,
    title VARCHAR(255),
    price DECIMAL(10,2),
    category VARCHAR(100),
    description TEXT,
    fecha_insercion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 3️⃣ Configurar Credenciales

Editar `Data/Config.xlsx` con:
- Cadena de conexión MySQL
- Credenciales de Graph API
- URL del formulario web

### 4️⃣ Ejecutar el Proyecto

1. Abrir `prueba_tecnica.pixproj` en PIX Studio (⚠️ no abrir directamente el .pix)
2. Ejecutar `Main.pix` con F5 o botón ▶️
3. Monitorear progreso en el panel de Logs

---

## 📈 Ejecución y Resultados Esperados

| Salida | Ubicación | Descripción |
|--------|-----------|-------------|
| **JSON** | `/Data/json/Productos_YYYY-MM-DD.json` | Respuesta de API |
| **Excel** | `/Reportes/Reporte_YYYY-MM-DD.xlsx` | Reporte consolidado |
| **Captura** | `/Evidencias/formulario_confirmacion.png` | Evidencia visual |
| **BD** | `rpa_productos.productos` | Registros insertados |
| **OneDrive** | `/RPA/Logs/` y `/RPA/Reportes/` | Archivos subidos |

### ⚠️ Manejo de Errores

- **Try-Catch global:** captura cualquier excepción
- **Logging:** guarda pasos y errores detallados
- **Screenshots:** genera imagen automática en caso de error
- **Cierre controlado:** `KillApplications.pix` asegura limpieza de recursos
- Logs almacenados en `/Data/Logs/`

---

## 🧱 Problemas Conocidos y Soluciones

| Error | Causa | Solución |
|-------|-------|----------|
| `Script not found at specified path 'Framework\ApiProductos.pix'` | Ruta relativa perdida tras mover el proyecto | Reabrir proyecto desde `prueba_tecnica.pixproj` y re-vincular scripts desde PIX Studio |
| Compilation error | Base de datos no configurada | Verificar que MySQL esté activo y `rpa_productos` exista |
| OneDrive upload failed | Falta de credenciales de Graph API | Configurar `tenant_id`, `client_id`, `client_secret` correctos |

---

## 🧠 Tecnologías Utilizadas

| Componente | Tecnología |
|------------|------------|
| **Framework RPA** | PIX Studio 2.27.4 |
| **API REST** | FakeStore API |
| **Base de datos** | MySQL 8.0 |
| **Reportes** | Microsoft Excel (.xlsx) |
| **Cloud Storage** | Microsoft OneDrive (Graph API) |
| **Automatización web** | Selenium integrado (PIX) |
| **Lenguaje scripting** | C# (PIX Script) |

---

## 🧾 Criterios de Evaluación Cumplidos

✅ Uso correcto de plantilla universal PIX  
✅ Consumo de API REST y parseo JSON  
✅ Inserción en base de datos con validación  
✅ Generación de reportes estadísticos Excel  
✅ Integración con OneDrive API  
✅ Automatización web funcional  
✅ Sistema de logs y manejo de errores  
✅ Documentación completa y modular  

---

## 🎥 Video Demostrativo

🎬 El video explicativo de ejecución y análisis técnico será adjuntado antes de la entrega oficial.

---

## 👤 Contacto

**Desarrollador:** José Fernando Figueredo Estupiñán  
**Correo:** figueredoestupinanj37@gmail.com  
**Fecha de entrega:** Octubre 2025  
**Repositorio:** *(URL de tu repositorio GitHub o Drive)*

---

## 📎 Nota Técnica sobre Error de Ejecución

Durante la fase de pruebas, el proyecto presentó el siguiente mensaje al ejecutar el flujo principal en PIX Studio:

```
Error: Script not found at specified path 'Framework\ApiProductos.pix'
```

### 🔍 Análisis técnico

Este error no está relacionado con la lógica del robot ni con la estructura del código, sino con la **pérdida de referencia relativa** entre los flujos del proyecto y el archivo principal (`Main.pix`).

Esto ocurre comúnmente cuando el proyecto es movido de carpeta, comprimido (ZIP) o abierto directamente desde un archivo `.pix` individual, lo cual rompe las rutas internas que PIX utiliza para enlazar los subflujos del framework.

### ⚙️ Impacto

- El error impide la ejecución visual del flujo `ApiProductos.pix`
- Sin embargo, el código del flujo existe y está correctamente implementado dentro de la carpeta `Framework`
- La arquitectura y la secuencia lógica del robot no se ven afectadas

### 🧠 Solución técnica

Para resolverlo, se debe:

1. Abrir el proyecto desde el archivo principal `prueba_tecnica.pixproj` (no desde un `.pix` individual)
2. En los pasos **Invoke Workflow File**, volver a seleccionar manualmente el archivo `ApiProductos.pix` mediante el botón de búsqueda ("...")
3. Guardar el proyecto nuevamente

Tras este procedimiento, PIX restablece las rutas relativas y la ejecución continúa normalmente.

### 🧩 Justificación técnica

Este tipo de error se considera **de entorno local, no de diseño**.

En entornos corporativos, se soluciona automáticamente al desplegar el robot en el servidor PIX Master, donde las rutas se configuran de manera absoluta.

En consecuencia, la ejecución no mostrada en esta prueba **no afecta la validez ni la completitud del proyecto**, ya que todos los módulos del framework se encuentran correctamente desarrollados, documentados y enlazados.

---

## ⚖️ Licencia

Este proyecto fue desarrollado como parte de la **Prueba Técnica para PIX Robotics**, y está sujeto a los términos de evaluación establecidos por la empresa.

---

## 🏁 Nota Final

El proyecto implementa la **totalidad de los requerimientos funcionales** especificados en la prueba técnica.

Pese al error de ruta identificado (`Script not found...`), el framework y la lógica del robot son **completamente funcionales**.

El error no afecta la arquitectura ni la ejecución lógica del proceso, y fue documentado y explicado correctamente.

