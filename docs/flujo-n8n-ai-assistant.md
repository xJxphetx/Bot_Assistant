# 🤖 AI Assistant – Flujo n8n para AG Electrónica

## Objetivo del flujo

Este workflow proporciona respuestas automáticas a consultas de usuarios mediante un frontend web (chat). El flujo se comunica con OpenAI (Assistant API) y estructura las respuestas según reglas del catálogo de AG Electrónica.

---

## Webhooks del flujo

| Webhook        | Ruta n8n                             | Descripción                             |
|----------------|--------------------------------------|-----------------------------------------|
| Ventas         | `/webhook/Sales`                     | Interacción de compra y productos       |
| Asesoramiento  | `/webhook/Advice`                    | Preguntas técnicas o sin producto claro |

---

## Flujo general (Ventas)

```text
Frontend → Webhook (/Sales) 
→ Crea Thread OpenAI 
→ Envía mensaje y reglas 
→ Ejecuta Run 
→ Espera resultado 
→ Extrae productos válidos (Stock + Precio)
→ Formatea respuesta
→ Regresa al frontend
→ Guarda en Supabase
```

---

## Flujo general (Asesoramiento)

```text
Frontend → Webhook (/Advice)
→ Crea Thread
→ Envia pregunta y contexto
→ Ejecuta Run y espera respuesta
→ Devuelve respuesta como texto puro
→ Registra sesión en Supabase
```

---

## Nodos importantes

| Nodo                 | Descripción                                               |
|----------------------|-----------------------------------------------------------|
| `Thread_ID`          | Crea un nuevo thread de OpenAI                            |
| `Thread_Message`     | Envía la pregunta del usuario                             |
| `Thread_Instructions`| Instrucciones detalladas con reglas para la IA           |
| `Thread_IA`          | Ejecuta el assistant                                       |
| `Thread_Answer`      | Extrae la respuesta generada por la IA                    |
| `Separate_Correct_Products` | Filtra productos válidos con CES, Stock y enlace   |
| `Final_Answer`       | Genera respuesta HTML lista para inyectar en el frontend  |
| `Answer_Adjust`      | Compila datos para guardar                                |
| `Insert_Data`        | Guarda la sesión en Supabase                              |
| `Respond to Webhook1`| Devuelve respuesta al navegador                           |

---

##  Reglas y validaciones del flujo

- Solo se muestran productos si tienen stock (`cantidadStock` extraído del HTML).
- Se extraen precios con y sin IVA.
- Enlace limpio y funcional generado dinámicamente.
- Se prohíbe el uso de ciertos términos como `IP67` (controlado en instrucciones).
- Registra la pregunta, respuesta y sesión en Supabase.

---

## Datos guardados en Supabase

| Campo        | Valor                                        |
|--------------|----------------------------------------------|
| sessionid    | ID único de usuario generada desde el frontend |
| Pregunta     | Texto enviado por el cliente                  |
| Respuesta    | Texto final generado por el flujo             |
| Fecha        | ISO timestamp                                |
| tipo         | `"chat"`                                     |
| origen       | `"webhook"`                                  |

---

## Cómo se conecta con el frontend

En el HTML del frontend (chat), el botón de compra llama:

```js
fetch('https://iaagelectronica1.app.n8n.cloud/webhook-test/Sales', {
  method: 'POST',
  body: JSON.stringify({ question: "¿Tienen sensores de temperatura?", sessionId: "cliente_123456" }),
})
```

---

## Archivos adicionales

- [📄 JSON del flujo completo](./n8n-flujos/AI_Assistant.json)
- Puedes importar este archivo en n8n usando "Import workflow".

---

## Autor

- **Desarrollador:** **@xJxphetx** - Julio de 2025
