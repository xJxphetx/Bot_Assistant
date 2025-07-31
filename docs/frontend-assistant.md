# 💻 Frontend Chat - AG Electrónica

## Objetivo

Este archivo `index.html` actúa como el frontend del asistente virtual de AG Electrónica. Simula un chatbot embebido en la web que permite a los usuarios interactuar con un flujo en n8n a través de Webhooks para ventas, asesoramiento y feedback.

---

## Estructura general

- Chat estilo burbuja (bot y usuario)
- Selección inicial:  
  - 🛒 Comprar → Flujo de ventas  
  - 🔍 Asesoramiento → Flujo técnico  
  - 🛠️ Soporte → Link a WhatsApp  
- Comunicación vía `fetch()` hacia n8n
- Soporte para adjuntar imágenes
- Evaluación de respuesta con 👍 / 👎

---

## Webhooks utilizados

| Acción               | Webhook conectado                                |
|----------------------|--------------------------------------------------|
| Compra               | `/webhook/Sales`                                 |
| Asesoramiento        | `/webhook/Advice`                                |
| Evaluación respuesta | `/webhook/satisfaction`                          |

> Todos los webhooks apuntan a la instancia segura: `https://iaagelectronica.cloud/webhook/...`

---

## Flujo de funcionamiento

1. El usuario recibe un mensaje inicial con opciones.
2. Elige "Comprar" o "Asesoramiento".
3. Se establece el `webhook` y el `chat_origin` (guardado en `localStorage`).
4. Puede escribir una pregunta y/o subir imagen.
5. El mensaje se acumula durante 5 segundos (por si escribe más).
6. Se envía el mensaje acumulado (y la imagen si aplica).
7. El bot responde.
8. El usuario califica la respuesta con "Sí" o "No".
9. Se registra en Supabase la satisfacción (opcional).

---

## Lógica avanzada

- **Envío de imagen:** Codificada en base64, subida a ImgBB, y enviada como URL a n8n.
- **Acumulación:** Si el usuario escribe varias veces antes de 5 segundos, se unen en un solo mensaje.
- **Preview de imagen:** Se muestra antes de enviar, con opción de eliminar.
- **Respuesta enriquecida:** Se parsea como HTML y se convierte en enlaces si hay URLs.
- **Evaluación:** El usuario puede indicar si fue útil la respuesta.
- **Reformulación:** Si dice que no fue útil, puede reformular o ir a WhatsApp.

---

## Envío de mensaje (fetch)

```js
fetch(webhook, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    question: "Texto acumulado del usuario",
    image: "https://...", // si hay imagen
    sessionId: "cliente_123456"
  })
})
```

- `sessionId` se genera automáticamente y se guarda en `localStorage`.
- `image` solo se envía si es válida y fue subida exitosamente a ImgBB.
- `question` es el texto acumulado por el usuario.

---

## Categorías al seleccionar "Comprar"

- 🔌 Componentes Electrónicos  
- 💻 Tarjetas de Desarrollo IoT / AI  
- 🔋 Fuentes, Baterías y Cautines  
- 💡 Iluminación LED  
- 🖨️ Impresión 3D  
- 📟 Instrumentación  
- 🚁 Drones y Refaccionamiento

Cada categoría lanza una sugerencia específica que guía al usuario.

---

## Feedback del usuario

Después de cada respuesta se muestra:

```html
¿Te ha sido útil esta respuesta?
👍 Sí     👎 No
```

- Si elige 👍 o 👎 se manda a `/webhook/satisfaction`
- Estructura:

```json
{
  "sessionId": "cliente_123456",
  "valor": "si" | "no",
  "origin": "Sales" | "Advice",
  "date": "2025-07-31T15:30:00.000Z"
}
```

---

## Diseño

- Responsive (`max-width: 700px`)
- Fondo degradado y contenedor blanco
- Vista previa de imagen en miniatura
- Animación de “escribiendo…” (tres puntos)
- Chat tipo Messenger

---

## Archivos relacionados

- `Index.html` – HTML completo del frontend
- `flujo-n8n-ai-assistant.md` – Flujo lógico de n8n
- `AI_Assistant.json` – Flujo importable para n8n

---

## 👨‍💻 Autor

- Desarrollador: **@xJxphetx** – Julio de 2025
