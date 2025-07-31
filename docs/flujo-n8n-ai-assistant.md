[flujo-n8n-ai-assistant-actualizado.md](https://github.com/user-attachments/files/21535850/flujo-n8n-ai-assistant-actualizado.md)
# 🤖 AI Assistant – Flujo n8n para AG Electrónica (versión nueva)

## Objetivo del flujo

Este workflow automatiza respuestas para ventas y asesoramiento técnico mediante un frontend web (chat). Utiliza OpenAI (Assistant API v2) bajo reglas específicas del catálogo de AG Electrónica, y registra interacciones en Supabase.

---

## Webhooks del flujo

| Webhook        | Ruta n8n                             | Descripción                             |
|----------------|--------------------------------------|-----------------------------------------|
| Ventas         | `/webhook/Sales`                     | Consultas relacionadas a productos y compras |
| Asesoramiento  | `/webhook/Advice`                    | Preguntas técnicas o sin producto claro |
| Satisfacción   | `/webhook/satisfaction`              | Evalúa la calidad de la respuesta del asistente |

---

## Flujo general (Ventas)

```text
Frontend → Webhook (/Sales)
→ Verifica si ya existe un thread en Supabase
   → Sí: reutiliza thread
   → No: crea uno nuevo y lo registra
→ Envía mensaje e imagen (si existe)
→ Inyecta instrucciones con reglas
→ Ejecuta Run
→ Espera resultado o reintenta si falla
→ Evalúa si hay productos válidos
→ Si hay productos:
     → Extrae URLs, precios y stock desde el sitio web
     → Genera respuesta enriquecida (producto, stock, precios, enlace)
→ Si no hay productos:
     → Devuelve respuesta IA en texto plano
→ Responde al frontend
→ Guarda sesión completa en Supabase
→ Si no hay stock: registra en tabla aparte
```

---

## Flujo general (Asesoramiento)

```text
Frontend → Webhook (/Advice)
→ Verifica existencia de thread
→ Crea uno nuevo si no existe
→ Envía mensaje (con o sin imagen)
→ Inyecta instrucciones específicas (solo usa campo CES)
→ Ejecuta Run y espera respuesta
→ Limpia referencias internas de OpenAI
→ Devuelve respuesta en texto plano
→ Registra sesión en Supabase
```

---

## Flujo general (Satisfacción)

```text
Frontend → Webhook (/satisfaction)
→ Clasifica respuesta como positiva ("si") o negativa
→ Guarda en la tabla correspondiente en Supabase:
   - `satisfaction_approved` o `satisfaction_negative`
→ Incluye sessionId, valor, fecha, origen y tipo
```

---

## Nodos importantes

| Nodo                    | Descripción                                                             |
|-------------------------|-------------------------------------------------------------------------|
| `Thread_ID`             | Crea un nuevo thread en OpenAI                                          |
| `SearchThread`          | Verifica si ya existe un thread para la sesión                         |
| `CorrectThread`         | Determina cuál thread usar (nuevo o existente)                         |
| `Thread_Message`        | Envía la pregunta y/o imagen del cliente                               |
| `Thread_Instructions`   | Instrucciones personalizadas (prohibiciones, formato, reglas)          |
| `Thread_IA`             | Ejecuta la IA con el thread configurado                                |
| `Thread_Answer`         | Extrae la respuesta generada                                            |
| `Evaluate_Text`         | Detecta si la respuesta tiene productos válidos o solo texto plano     |
| `Separate_Correct_Products` | Extrae y valida productos con CES, stock y enlace limpio            |
| `Get_Correct_URLs`      | Corrige y transforma URLs en enlaces utilizables                       |
| `Extract_Price`, `Extract_Stock` | Scrapea la web de AG Electrónica para obtener datos reales   |
| `Final_Answer`          | Estructura respuesta completa y enriquecida                            |
| `IA_Answer1`            | Alternativa si no hay productos; devuelve solo texto de IA             |
| `Answer_Adjust`         | Prepara datos finales para guardar                                     |
| `Insert_Data`           | Guarda la sesión en `Compras_prueba`                                   |
| `NoStock`               | Guarda la sesión en `stock` si no se encontraron productos disponibles |
| `Negative_Satisfaction`, `Approved_Satisfaction` | Guardan feedback de clientes                |

---

## Reglas y validaciones del flujo

- Se usan solo productos con **stock** confirmado.
- No se muestran precios desde la base de datos, solo desde el HTML del sitio.
- Los productos se presentan solo si tienen coincidencia real.
- Se evita el uso de Markdown en los enlaces (sin corchetes ni paréntesis).

---

## Datos guardados en Supabase

### Compras y asesoramiento (`Compras_prueba`, `Asesoramiento_prueba`)

| Campo        | Valor                                       |
|--------------|---------------------------------------------|
| sessionid    | ID único por cliente                        |
| Pregunta     | Texto enviado por el cliente                |
| Respuesta    | Texto final mostrado al cliente             |
| Fecha        | Timestamp ISO                               |
| tipo         | `"chat"`                                    |
| origen       | `"webhook"`                                 |

### Thread (`threadsid`)

Registra el ID de conversación (thread_id) para evitar duplicados por sesión.

### Satisfacción (`satisfaction_approved` / `satisfaction_negative`)

| Campo      | Valor                                          |
|------------|------------------------------------------------|
| SessionID  | ID de la sesión respondida                     |
| Valor      | "si" o "no" según opinión del cliente          |
| Origin     | `"webhook"`                                    |
| Type       | `"satisfaction"`                               |
| Date       | Fecha del envío de opinión                     |

---

## Conexión con el frontend

El chat HTML envía la pregunta así:

```js
fetch('https://iaagelectronica.cloud/webhook/Sales', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    question: "¿Tienen sensores de temperatura?",
    image: null, // o URL de imagen si aplica
    sessionId: "cliente_123456"
  })
})
```

---

## Archivos adicionales

- [📄 JSON del flujo nuevo](./n8n-flujos/AI_Assistant.json)
- Puede importarse en n8n desde “Import workflow”.

---

## Autor

- **Desarrollador:** **@xJxphetx** – Julio 2025
