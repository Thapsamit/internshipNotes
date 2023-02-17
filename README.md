

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
 
