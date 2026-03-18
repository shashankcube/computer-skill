# `send_notification` — Schema Reference

Send notification to users or groups. By default, notification priority will be marked as "Important". Emails are sent for unread "Important" notifications. Once the workflow is deployed, any receiver can go to notification preferences and change the priority to "Others" to stop receiving emails.

## Input Port: `input`

- **`receivers`** (`[]id`) **REQUIRED** — The users or groups who will receive the notification.
  - ID type: `devu`, `group`
- **`body`** (`rich_text`) **REQUIRED** — The body of the notification.
- **`title`** (`rich_text`) **REQUIRED** — The title of the notification.
- **`content_template_id`** (`id`) — The content template for the notification.
  - ID type: `notification_content_template`
  - UI: hidden
- **`linked_object`** (`id`) — The object which will open on clicking the notification.
  - ID type: `account`, `capability`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `runnable`, `ticket`, `conversation`

## Output Port: `output`

No output fields defined (empty schema).
