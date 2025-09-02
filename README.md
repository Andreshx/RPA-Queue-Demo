# RPA Queue Demo – Power Automate

Este proyecto es un **demo práctico de RPA (Automatización de Procesos Robóticos)** implementado con **Microsoft Power Automate**.  
El flujo simula un sistema de **colas de trabajo** que recibe solicitudes desde correos electrónicos, las encola en **Microsoft Lists/SharePoint**, y luego las procesa secuencialmente con lógica de control de errores e integración con un servicio externo (API de clima).

---

## 🚀 Funcionalidad

1. **Captura de solicitudes**  
   - Cuando llega un correo a una carpeta específica de Outlook, se genera automáticamente un nuevo ítem en la lista de SharePoint (la cola).

2. **Gestión de cola**  
   - Los ítems se ordenan por prioridad y fecha de creación.  
   - Solo se procesa un ítem a la vez (control de concurrencia activado).  

3. **Procesamiento**  
   - Se consulta una API externa (Open-Meteo) para obtener datos de clima.  
   - Los resultados se guardan en la cola con estado **Culminado**.  

4. **Manejo de errores**  
   - Si el procesamiento falla, el ítem pasa a estado **Fallido**.  
   - Existe lógica para **reencolar** la tarea si aún no supera el número máximo de intentos.

---

## 📂 Estructura de la solución

La solución exportada contiene:

- **Flows (Power Automate)**  
  - *Correo a RequestsQueue*: ingesta de correos → cola.  
  - *Despachador-RequestsQueue*: procesamiento de cola, API y actualización de estados.  

- **Microsoft List / SharePoint**  
  - Lista `RequestsQueue` con columnas:
    - RequestId  
    - Estado (Pendiente, En progreso, Culminado, Fallido)  
    - Prioridad  
    - Intentos  
    - Resultado / Detalles  

- **Variables de entorno**  
  Configuradas para facilitar la importación en otro tenant:
  - `SiteUrl` → URL base de SharePoint.  
  - `ListName` → Nombre de la lista de la cola.  
  - `MailFolder` → Carpeta de Outlook para monitorear correos.  
  - `WeatherApiUrl` → Endpoint base de la API de clima.

---

## 🔧 Requisitos previos

- Licencia de **Power Automate** (conectores premium si aplica).  
- Acceso a **SharePoint Online** y **Outlook 365**.  
- API pública (ejemplo: [Open-Meteo](https://open-meteo.com/)).

---

## 📥 Instalación

1. Descargar la solución desde [`/export/RPA-Queue-Demo.zip`](./export/RPAQueueDemo_1_0_0_1_managed.zip).  
2. En **Power Apps / Power Automate**, ir a:  
   `Soluciones > Importar solución`.  
3. Durante la importación, asignar valores a las **variables de entorno**:
   - `SiteUrl`: URL de tu sitio SharePoint.  
   - `ListName`: nombre real de tu lista.  
   - `MailFolder`: carpeta de Outlook donde llegarán los correos.  
   - `WeatherApiUrl`: `https://api.open-meteo.com/v1/forecast`.  

---

## 📊 Ejemplo de ejecución

1. Enviar un correo a la carpeta configurada.  
2. Ver el nuevo ítem en la lista `RequestsQueue` en estado **Pendiente**.  
3. El despachador procesa el ítem → consulta clima → actualiza estado a **Culminado** con resultados.  
4. Si falla, el estado pasa a **Fallido** (con detalle del error) o se reencola según intentos.

---

## 🎯 Objetivo

Este proyecto demuestra:

- Uso de **colas de trabajo simuladas** con SharePoint.  
- Integración de **Outlook, SharePoint y API REST** en Power Automate.  
- Control de **concurrencia y reintentos**.  
- Buenas prácticas con **variables de entorno** para portabilidad.

---

## 📌 Notas

- Puedes adaptarlo fácilmente para escenarios reales: atención a tickets, procesamiento de documentos, integraciones con ERP, etc.

---
