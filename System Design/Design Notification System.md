# _Design a Notification System_

The system should allow different services such as Order, Payment, or Authentication to send notifications to users through channels such as push notifications, email, SMS, and in-app notifications.

The system should support high throughput, retries, user preferences, scheduled notifications, and reliable delivery.

### 1. High-level design

I would divide the system into the following main components:

- Notification Service: Accepts notification requests and decides which channels should be used.
- Message Queue: Kafka or another durable queue for asynchronous processing.
- Channel Workers: Separate workers for email, SMS, push, and other channels.
- Template Service: Stores and renders notification templates.
- Preference Service: Stores user notification preferences.
- Database: Stores notification metadata, delivery status, preferences, templates, and idempotency information.
- External Providers: Services such as email, SMS, and push notification providers.

The high-level flow would be:

```
Order / Payment / Auth Service
              |
              v
      Notification Service
              |
       +------+------+
       |             |
 Preferences      Templates
       |             |
       +------+------+
              |
              v
            Kafka
              |
      +-------+-------+
      |       |       |
    Email   Push     SMS
   Worker   Worker   Worker
      |       |       |
      v       v       v
   Provider Provider Provider
```

I would make the system asynchronous because the calling service should not wait for an external notification provider to deliver the notification.

### 2. API design

I would expose an endpoint such as:

- `POST /api/notifications`

The request could contain:

```json
{
  "user_id": "123",
  "template_id": "order_shipped",
  "channels": ["email", "push"],
  "data": {
    "order_id": "ORD123"
  },
  "idempotency_key": "order-ORD123-shipped"
}
```

The API would return a notification_id and an accepted status instead of waiting for the actual delivery.

I would also have an endpoint such as:

- `GET /api/notifications/{notificationId}`

to retrieve the notification and delivery status.

### 3. Notification flow

Suppose the Order Service publishes an OrderShipped event.

The Notification Service consumes the event and checks the user's preferences. It then selects the appropriate channels and retrieves the corresponding template.

For example:

```
OrderShipped
     |
     v
Notification Service
     |
     +----> User prefers Email? ----> Email Job
     |
     +----> User prefers Push? -----> Push Job
     |
     +----> User prefers SMS? ------> SMS Job
```

These jobs are published to Kafka, and channel-specific workers process them asynchronously.

This prevents a slow email or SMS provider from blocking the Order Service.

### 4. Database design

I would have tables roughly like:

Notification

| **Field**       | **Description**         |
| --------------- | ----------------------- |
| notification_id | Unique notification ID  |
| user_id         | Recipient               |
| template_id     | Template used           |
| event_id        | Source event            |
| priority        | HIGH / NORMAL / LOW     |
| created_at      | Creation time           |
| scheduled_at    | Optional scheduled time |

Delivery

| **Field**           | **Description**         |
| ------------------- | ----------------------- |
| delivery_id         | Unique delivery ID      |
| notification_id     | Notification reference  |
| channel             | EMAIL / SMS / PUSH      |
| status              | PENDING / SENT / FAILED |
| attempt_count       | Number of attempts      |
| provider_message_id | Provider reference      |
| last_attempt_at     | Last attempt time       |

I would also maintain user preferences separately.

For example:

```
user_id | channel | notification_type | enabled
123     | EMAIL   | ORDER_UPDATE      | true
123     | SMS     | ORDER_UPDATE      | false
123     | PUSH    | ORDER_UPDATE      | true
```
