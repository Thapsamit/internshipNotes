

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



How to measure average time ?

```python
import datetime

# Create a list of datetime.time objects
time_list = [datetime.time(hour=10, minute=30),
             datetime.time(hour=11, minute=45),
             datetime.time(hour=12, minute=0),
             datetime.time(hour=13, minute=15)]

# Convert the datetime.time objects to datetime.datetime objects with the same date
datetime_list = [datetime.datetime.combine(datetime.date.today(), t) for t in time_list]

# Compute the average datetime object
avg_datetime = datetime.datetime.fromtimestamp(sum(map(datetime.datetime.timestamp, datetime_list)) / len(datetime_list))

# Extract the time part of the average datetime object
avg_time = avg_datetime.time()

# Print the average time
print(avg_time)


```



```python
@api_view(['POST'])
def GetAllSessions(request):
    sess_objs = Session.objects.all()
    print(sess_objs)
    sess_data = MandalaSessionSerializer(sess_objs,many=True).data 
    return Response({"session_data":sess_data},status=status.HTTP_200_OK)
```
