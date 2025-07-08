# 💻 Frontend Chat - AG Electrónica

##  Objetivo

Este archivo `index.html` actúa como el frontend del asistente virtual de AG Electrónica. Simula un chatbot embebido en la web que permite a los usuarios interactuar con un flujo en n8n a través de Webhooks.

---

##  Estructura general

- Chat simulado entre usuario y asistente (estilo burbuja)
- Botón para seleccionar:
  - 🛒 Comprar (flujo de ventas)
  - 🔍 Asesoramiento (flujo técnico)
- Comunicación vía `fetch` hacia webhooks de n8n

---

## 🌐 Webhooks utilizados

| Acción               | Webhook conectado                          |
|----------------------|--------------------------------------------|
| Compra               | `/webhook/Sales`           |
| Asesoramiento        | `/webhook/Advice`              |

> ⚠️ Nota: Las URLs apuntan a la instancia pública de n8n (cloud). Puedes reemplazarlas por tus rutas locales o de producción si es necesario.

---

##  Flujo de funcionamiento

1. Al cargar la página, se muestra un mensaje de bienvenida del asistente.
2. El usuario selecciona "Comprar" o "Asesoramiento".
3. Según la opción, se habilita el input del chat y se establece la variable `webhook`.
4. El mensaje se envía mediante `fetch()` a n8n.
5. La respuesta se renderiza en el DOM como mensaje del bot.
6. Se ofrece feedback de “¿fue útil?” con botones 👍 / 👎.

---

##  Envío de mensaje

```js
fetch(webhook, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ question: pregunta, sessionId })
})
```

- `sessionId`: Persistente con `localStorage`.
- `question`: Texto ingresado por el usuario.
- `RespuestaFinal`: Campo esperado como respuesta desde n8n.

---

##  Diseño

- Totalmente responsive (`max-width: 700px`)
- Colores claros y botones interactivos
- Estilo de burbuja (sin "puntas" si se desactiva el CSS correspondiente)

---

##  Archivos relacionados

- `index.html` – Este archivo
- [📄 flujo-n8n-ai-assistant.md](./flujo-n8n-ai-assistant.md)
- [📄 AI_Assistant.json](../n8n-flujos/AI_Assistant.json)

---

##  Autor

- Desarrollador: **@xJxphetx** - Julio de 2025
