# 🤖 AG Electrónica – Asistente Virtual

Bienvenido al repositorio del **Asistente Virtual de AG Electrónica**, una integración entre un frontend interactivo y flujos backend usando **n8n + OpenAI**.

---

##  Demo

![Chatbot en acción]
![image](https://github.com/user-attachments/assets/8cbf0a13-4d3c-41b2-b955-2395bbf22540)

> Interfaz tipo chat 100% responsive para ayudar a tus clientes en tiempo real.

---

##  Tecnologías

-  [n8n.io](https://n8n.io) – Automatización backend con lógica sin código
-  OpenAI Assistants v2 – Motor de respuesta basado en IA
-  HTML + JS + CSS – Frontend embebido simple y efectivo
- ☁ Supabase – Registro de logs y sesiones

---

##  Estructura del proyecto

```bash
Bot_Assistant/
├── docs/
│   ├── flujo-n8n-ai-assistant.md       # Documentación del flujo backend
│   └── frontend-assistant.md           # Documentación del frontend
├── n8n-flujos/
│   └── AI_Assistant_sanitized.json     # Export JSON del flujo n8n (sin datos sensibles)
├── index.html                          # Código del chatbot web
└── README.md                           # Este archivo
```

---

##  ¿Cómo funciona?

###  Usuario:
- Abre la web → Interactúa con el chatbot
- Pregunta por productos o soporte

###  Asistente:
- Envia la consulta a un webhook de n8n
- n8n → OpenAI → lógica de validación + productos disponibles
- Devuelve una respuesta estructurada y útil

---

## 📝 Documentación

-  [Flujo n8n AI Assistant](./docs/flujo-n8n-ai-assistant.md)
-  [Frontend del asistente](./docs/frontend-assistant.md)
-  [JSON del flujo (seguro)](./n8n-flujos/AI_Assistant_sanitized.json)

---

##  Reglas especiales del asistente (resumen)

- Solo responde con productos en stock
- No menciona precios explícitos (solo con IVA/desglose si lo calculas)
- Evita términos como "IP67", usa “sumergible” o “para exteriores”
- No inventa enlaces: se construyen basados en `NUM_PARTE`

---

##  Créditos

Desarrollado por xJxphetx



