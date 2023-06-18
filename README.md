## Web sockets 

- Consumers.py file

# consumers.py

```python
import json
from channels.generic.websocket import AsyncWebsocketConsumer
from django.contrib.auth import get_user_model
from your_app.models import Notification

class NotificationConsumer(AsyncWebsocketConsumer):
    async def connect(self):
        # Get the email from the URL or headers, authenticate the user, and establish the WebSocket connection
        # Example: authentication using URL query parameter
        email = self.scope['url_route']['kwargs']['email']
        user = get_user_model().objects.get(email=email)
        await self.accept()

        # Subscribe the user to their corresponding notification channel
        await self.channel_layer.group_add(
            str(user.id),
            self.channel_name
        )

    async def disconnect(self, close_code):
        # Unsubscribe the user from their notification channel
        email = self.scope['url_route']['kwargs']['email']
        user = get_user_model().objects.get(email=email)
        await self.channel_layer.group_discard(
            str(user.id),
            self.channel_name
        )

    async def notification_update(self, event):
        # Send notification update to the WebSocket connection
        await self.send(text_data=json.dumps(event['data']))

```
