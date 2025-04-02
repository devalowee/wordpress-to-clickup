
# 📖 ClickUp Webhook Integration

## 📌 Description
This plugin connects FluentForms with ClickUp via a webhook, allowing automatic creation of tasks in ClickUp based on form submissions.

## 📦 Installation
1. Upload the plugin folder to:
```
/wp-content/plugins/
```
2. Activate **ClickUp Webhook Integration** from the Plugins menu in WordPress.

## ⚙️ Plugin Configuration
In the **ClickUp Integration** menu, configure the following:

- **API Token**: Token generated from your ClickUp account.
- **List ID**: ID of the ClickUp list.
- **Initial Task Status**: Initial status for the created task.
- **Webhook Security Key**: Secret key to validate the webhook.

Click **Save Changes** to save your settings.

## 🚨 New Security Requirement (Security Key in Header)
It is mandatory to include a security key in the HTTP headers of the webhook:

- **Header Name**: `X-Clickup-Webhook-Key`
- **Value**: Must match the configured key in the plugin.

## 🌐 Webhook Endpoint
Use the following URL for your webhook:

```
https://your-website.com/wp-json/clickup/v1/webhook/
```

Required headers:
```json
{
  "X-Clickup-Webhook-Key": "your_secret_key"
}
```

## ✅ Mandatory JSON Fields
- **source**: (Required) Task title.
- **date**: (Optional) Associated date.
- **name**: (Optional) Associated person's name.

Example:
```json
{
  "source": "Invitation Request",
  "date": "2024-04-02",
  "name": "Alex Garcia"
}
```

## 🔐 Security Key Validation
If the provided key doesn't match, the webhook returns an HTTP `401 Unauthorized` error and logs the incident.

## 📜 Logs
Logs are stored in:

```
/wp-content/clickup-webhook.log
```

Logs include received requests, validation failures, tasks created, and communication errors.

## 🔧 Internal Workflow
1. Receives POST request.
2. Validates Security Key.
3. Processes JSON data and creates task in ClickUp.
4. Returns a JSON response indicating success or failure.

## 🚧 CURL Example
```bash
curl -X POST https://your-website.com/wp-json/clickup/v1/webhook/ \
-H "Content-Type: application/json" \
-H "X-Clickup-Webhook-Key: your_secret_key" \
-d '{
  "source": "Test Request",
  "date": "2024-04-02",
  "name": "Test via CURL"
}'
```

## 🔗 Repository
[https://github.com/devalowee/wordpress-to-clickup](https://github.com/devalowee/wordpress-to-clickup)
