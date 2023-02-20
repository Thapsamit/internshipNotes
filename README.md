

## use below to store session data in redux
```js
let obj = {}
let data = {
    "session": {
        "session_id": 6,
        "numbered_level": 1,
        "session_video_id": "",
        "session_audio": "",
        "session_start_heading": "",
        "session_start_subtext": "",
        "session_end_heading": "",
        "session_end_subtext": ""
    },
    "user_session": {
        "user_session_uuid": "d7550d44-6be4-4d5a-b2c0-adc31b22746c"
    }
}

let newstate = {...obj,"sessiondata":data}
console.log(newstate)
```
 
 
 ## Whatsapp templates
 
```json 
{
    "recipient": {
        "id": "whatsapp:+1234567890"
    },
    "message": {
        "template": {
            "namespace": "your-namespace",
            "name": "your-template-name",
            "language": {
                "code": "en"
            },
            "components": [
                {
                    "type": "header",
                    "parameters": [
                        {
                            "type": "text",
                            "text": "Order Confirmation"
                        }
                    ]
                },
                {
                    "type": "body",
                    "parameters": [
                        {
                            "type": "text",
                            "text": "Hi {{customer_name}},\n\nThank you for your order! Your order number is {{order_number}} and your total is {{order_total}}. We will notify you once your order has been shipped.\n\nThanks,\nYour Store Name"
                        }
                    ]
                }
            ]
        },
        "parameters": [
            {
                "type": "text",
                "text": "John Doe",
                "name": "customer_name"
            },
            {
                "type": "text",
                "text": "123456",
                "name": "order_number"
            },
            {
                "type": "text",
                "text": "$50.00",
                "name": "order_total"
            }
        ]
    }
}

```
