# 🚗 Agente IA de Ventas — Automóviles Élite Valladolid

Agente conversacional de ventas construido con n8n que automatiza 
la captación de leads, cualificación, envío de ofertas y seguimiento 
de clientes a través de WhatsApp y email.

## 🧠 ¿Qué hace?

- Responde automáticamente a clientes por WhatsApp
- Cualifica leads extrayendo nombre, email, coche de interés y cita
- Envía un email profesional con ficha técnica, precio y confirmación de cita
- Realiza seguimiento automático diario a leads que no han respondido
- Guarda y actualiza toda la información en una base de datos en tiempo real

## ⚙️ Stack tecnológico

| Herramienta | Uso |
|-------------|-----|
| n8n | Orquestación de workflows |
| Groq (LLaMA 3.1) | Agente conversacional y extractor de datos |
| WhatsApp Cloud API | Canal de comunicación con el cliente |
| Gmail API | Envío de emails de oferta y seguimiento |
| Supabase | Base de datos de leads y conversaciones |

## 🔄 Workflows

### 1. Agente IA Ventas (Principal)
Flujo activado por webhook de WhatsApp que gestiona toda la 
conversación con el cliente.

**Ciclo de venta:**
1. Cliente saluda por WhatsApp
2. El agente presenta el catálogo y precios
3. Recoge nombre, email y propone cita en un solo mensaje
4. Cuando tiene los 4 datos (nombre + email + día + hora) dispara el email
5. Guarda el lead cualificado en Supabase

### 2. Seguimiento Diario
Flujo programado que se ejecuta cada día a las 9:00.

**Ciclo de seguimiento:**
1. Busca leads con oferta enviada hace más de 24h sin respuesta
2. Envía WhatsApp de recordatorio personalizado
3. Envía email de seguimiento con oferta del vehículo
4. Actualiza el estado del lead en Supabase

## 🗄️ Estructura de Supabase

### Tabla `leads`
| Campo | Tipo | Descripción |
|-------|------|-------------|
| telefono | text | Identificador único del lead |
| nombre | text | Nombre completo del cliente |
| email | text | Correo electrónico |
| coche_interes | text | Modelo de vehículo consultado |
| estado | text | Estado del lead |
| estado_seguimiento | text | OFERTA_ENVIADA / OFERTA_COMPLETADA |
| email_enviado | boolean | Si se ha enviado la ficha |
| ultima_actualizacion | timestamp | Última interacción |

### Tabla `conversaciones`
| Campo | Tipo | Descripción |
|-------|------|-------------|
| telefono | text | Identificador del lead |
| historial | text | JSON con el historial completo |
| ultima_actualizacion | timestamp | Última actualización |

## 📸 Capturas

### Conversación WhatsApp
El agente recoge todos los datos en pocos mensajes y confirma 
la cita automáticamente.

### Email de confirmación
Email profesional con diseño corporativo que incluye:
- Cita confirmada con día y hora
- Ficha técnica del vehículo
- Precio y cuota mensual
- Botón de contacto directo por WhatsApp

## 🚀 Cómo importar los workflows

1. Descarga los archivos JSON de este repositorio
2. Abre tu instancia de n8n
3. Ve a **Workflows → Import from file**
4. Importa cada JSON por separado
5. Configura las credenciales:
   - WhatsApp Cloud API
   - Gmail OAuth2
   - Supabase API
   - Groq API

## 📋 Variables de entorno necesarias

- `WHATSAPP_PHONE_NUMBER_ID` — ID del número de WhatsApp Business
- `GROQ_API_KEY` — Clave de la API de Groq
- `SUPABASE_URL` — URL de tu proyecto Supabase
- `SUPABASE_API_KEY` — Clave de Supabase
- `GMAIL_OAUTH` — Credenciales OAuth2 de Gmail

## 👨‍💻 Autor

Ezel Alexander Duque Arias
Desarrollado como proyecto de prácticas de automatización con IA.
