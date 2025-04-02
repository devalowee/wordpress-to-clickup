
# 📖 ClickUp Webhook Integration

## 📌 Descripción
Este plugin conecta FluentForms con ClickUp mediante un webhook, permitiendo crear automáticamente tareas en ClickUp desde las respuestas de un formulario.

## 📦 Instalación
1. Sube la carpeta del plugin a la ruta:
```
/wp-content/plugins/
```
2. Activa **ClickUp Webhook Integration** desde el menú de Plugins en WordPress.

## ⚙️ Configuración del Plugin
En el menú **ClickUp Integration** debes configurar:

- **API Token**: Token generado desde tu cuenta ClickUp.
- **List ID**: ID de la lista de ClickUp.
- **Initial Task Status**: Estado inicial de la tarea.
- **Webhook Security Key**: Clave secreta para validar el webhook.

Presiona **Save Changes** para guardar tu configuración.

## 🚨 Nuevo Requisito de Seguridad (Security Key en Header)
Es obligatorio incluir una clave de seguridad en los headers HTTP del webhook:

- **Nombre del Header**: `X-Clickup-Webhook-Key`
- **Valor**: Debe coincidir con la clave configurada en el plugin.

## 🌐 Endpoint del Webhook
Usa esta URL en tu webhook:

```
https://tu-sitio-web.com/wp-json/clickup/v1/webhook/
```

Headers obligatorios:
```json
{
  "X-Clickup-Webhook-Key": "tu_clave_secreta"
}
```

## ✅ Campos obligatorios en el JSON enviado
- **source**: (Obligatorio) Título de la tarea.
- **date**: (Opcional) Fecha asociada.
- **name**: (Opcional) Nombre asociado.

Ejemplo:
```json
{
  "source": "Solicitud de invitaciones",
  "date": "2024-04-02",
  "name": "Alex García"
}
```

## 🔐 Validación del Security Key
Si la clave no coincide, el webhook devuelve un error HTTP `401 Unauthorized` y registra el error en los logs.

## 📜 Registro (Logs)
Los logs se encuentran en:

```
/wp-content/clickup-webhook.log
```

Incluyen solicitudes recibidas, validaciones fallidas, tareas creadas y errores de comunicación.

## 🔧 Funcionamiento interno
1. Recibe una solicitud POST.
2. Valida el Security Key.
3. Procesa el JSON y crea la tarea en ClickUp.
4. Devuelve una respuesta JSON con éxito o error.

## 🚧 Ejemplo de llamada CURL
```bash
curl -X POST https://tu-sitio-web.com/wp-json/clickup/v1/webhook/ \
-H "Content-Type: application/json" \
-H "X-Clickup-Webhook-Key: tu_clave_secreta" \
-d '{
  "source": "Solicitud prueba",
  "date": "2024-04-02",
  "name": "Prueba desde CURL"
}'
```

## 🔗 Repositorio
[https://github.com/devalowee/wordpress-to-clickup](https://github.com/devalowee/wordpress-to-clickup)
