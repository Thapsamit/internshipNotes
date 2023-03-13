

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





```python
class SubLevelSerializer(serializers.ModelSerializer):
    class Meta:
        model = SubLevel 
        fields = "__all__"

class MandalaSessionSerializer(serializers.ModelSerializer):
    sublev = SubLevelSerializer(source='sublevel_id') # source is basically the column name in which foreignkey is specified
    class Meta:
        model = Session
        fields = ['session_uuid','numbered_level', 'session_audio','sublev']
        
```



```python
from django.db.models import Q

# Assume your model is named Submission and has fields "user", "date", and "data"
# We'll assume "date" is a DateTimeField

# First, get the current submission for the given date
current_submission = Submission.objects.filter(
    user=user,
    date__date=date  # filter by the date portion of the datetime field
).latest('date')

# Next, get the previous submission (if it exists)
previous_submission = Submission.objects.filter(
    Q(user=user) & Q(date__lt=current_submission.date)
).latest('date')


```


## The below serializes a single column or method field

```python 

class ProgressSerializer(serializers.ModelSerializer):
    progress_outcomes = serializers.SerializerMethodField()

    def get_progress_outcomes(self, obj):
        return [outcome.outcome for outcome in obj.progress_outcomes.all()]
    
    class Meta:
        model = Progress
        fields = ['retention_duration','av_ratio','ih_ratio','resistance','restless','practice_time','progress_outcomes']
```


# range based filtering in react

```js

import { useSelector } from 'react-redux';
import { useState } from 'react';

function FilteredRecords() {
  const records = useSelector((state) => state.records); // Assuming your records are stored in the Redux store
  const [startDate, setStartDate] = useState('');
  const [endDate, setEndDate] = useState('');

  const handleStartDateChange = (event) => {
    setStartDate(event.target.value);
  };

  const handleEndDateChange = (event) => {
    setEndDate(event.target.value);
  };

  const filteredRecords = records.filter((record) => {
    if (startDate && endDate) {
      return record.date >= startDate && record.date <= endDate;
    } else if (startDate) {
      return record.date >= startDate;
    } else if (endDate) {
      return record.date <= endDate;
    } else {
      return true;
    }
  });

  return (
    <div>
      <div>
        <label htmlFor="start-date">Start Date:</label>
        <input
          type="date"
          id="start-date"
          value={startDate}
          onChange={handleStartDateChange}
        />
      </div>
      <div>
        <label htmlFor="end-date">End Date:</label>
        <input
          type="date"
          id="end-date"
          value={endDate}
          onChange={handleEndDateChange}
        />
      </div>
      <ul>
        {filteredRecords.map((record) => (
          <li key={record.id}>
            {record.title} ({record.date})
          </li>
        ))}
      </ul>
    </div>
  );
}

```

