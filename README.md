# 🤖 AG Electrónica – Asistente Virtual

Bienvenido al repositorio del **Asistente Virtual de AG Electrónica**, una solución integrada que conecta una interfaz web interactiva con flujos backend automáticos en **n8n**, potenciados por **OpenAI Assistants v2**.

---

##  Demo de la interfaz

![Chatbot en acción](https://user-images.githubusercontent.com/demo-placeholder.gif)
![image](https://github.com/user-attachments/assets/8cbf0a13-4d3c-41b2-b955-2395bbf22540)

> Interfaz tipo chat optimizada para asesoramiento técnico, soporte y ventas. Compatible con imágenes, botones dinámicos y respuesta por IA.

---

##  Tecnologías utilizadas

- [n8n.io](https://n8n.io) – Automatización backend
- OpenAI Assistants v2 – Motor conversacional
- HTML + JS + CSS – Interfaz personalizada
- Supabase – Registro de sesiones e interacciones
- ImgBB – Carga temporal de imágenes de usuario

---

##  Estructura del proyecto

```bash
Bot_Assistant/
├── docs/
│   ├── flujo-n8n-ai-assistant.md       # Documentación del flujo backend
│   └── frontend-assistant.md           # Documentación del frontend
├── n8n-flujos/
│   └── AI_Assistant.json               # Flujo n8n actualizado
├── index.html                          # Interfaz del chatbot (optimizada)
├── image.png                           # Captura de la interfaz en acción
└── README.md                           # Este archivo
```
---

## ¿Cómo funciona?
### 1. El usuario accede a la web:
    - Selecciona “Asesoramiento” o “Compra”
    - Puede subir una imagen (opcional) y enviar su mensaje

### 2. El backend con n8n:
    - Redirige al flujo correcto vía webhook
    - Procesa la imagen (si aplica)
    - Consulta OpenAI + bases de datos (stock, productos, sesiones)
    - Registra logs y devuelve respuesta inteligente

---

##  Documentación

-  [Flujo n8n AI Assistant](./docs/flujo-n8n-ai-assistant.md)
-  [Frontend del asistente](./docs/frontend-assistant.md)
-  [JSON del flujo](./n8n-flujos/AI_Assistant.json)

---

## Bases de datos utilizadas

Este asistente utiliza las siguientes tablas en **Supabase** para registrar interacciones, evaluar satisfacción y gestionar procesos automatizados:

| Tabla                  | Descripción                                                                 |
|------------------------|------------------------------------------------------------------------------|
| `Asesoramiento`         | Guarda preguntas, respuestas y tipo de asesoría técnica de cada sesión.     |
| `Compra`                | Registra consultas relacionadas con compras, incluyendo tipo, origen y respuesta. |
| `stock`                 | Consulta disponibilidad de productos con base en la sesión y pregunta.       |
| `satisfaction_approved` | Almacena evaluaciones positivas de satisfacción del usuario.                |
| `satisfaction_negative` | Almacena evaluaciones negativas de satisfacción del usuario.                |
| `pendingmessages`       | Guarda mensajes en espera o no respondidos con su respectivo timestamp.     |
| `threadsId`             | Relaciona las sesiones con hilos (threads) para seguimiento.                |

Cada tabla se conecta al flujo de **n8n** para enriquecer la experiencia conversacional, registrar el historial y automatizar tareas de soporte y ventas.

---

## Créditos
### Desarrollado por @xJxphetx – Julio 2025
---
