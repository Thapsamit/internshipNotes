

# PWA Notes:-

- feels like native apps 
- can create it using vanilla js
- ability for push notifcation, offline access


## Create a Manifest file:-
- Like description of app like name,icon 
- require manifest.json 

```json
{
    "name":"Food Ninja",
    "short_name":"FoodNinja", // app name
    "start_url":"/index.html", // file to be open
    "display":"standalone", // this doesn't show address bar at the app and works like a native app
    "background_color":"#FFE9D2",
    "theme_color":"#FFE1C4",
    "orientation":"portrait-primary",
    // icons for different mobiles
    "icons": [
        {
          "src": "/img/icons/icon-72x72.png",
          "type": "image/png",
          "sizes": "72x72"
        },
        {
          "src": "/img/icons/icon-96x96.png",
          "type": "image/png",
          "sizes": "96x96"
        },
        {
          "src": "/img/icons/icon-128x128.png",
          "type": "image/png",
          "sizes": "128x128"
        },
        {
          "src": "/img/icons/icon-144x144.png",
          "type": "image/png",
          "sizes": "144x144"
        },
        {
          "src": "/img/icons/icon-152x152.png",
          "type": "image/png",
          "sizes": "152x152"
        },
        {
          "src": "/img/icons/icon-192x192.png",
          "type": "image/png",
          "sizes": "192x192"
        },
        {
          "src": "/img/icons/icon-384x384.png",
          "type": "image/png",
          "sizes": "384x384"
        },
        {
          "src": "/img/icons/icon-512x512.png",
          "type": "image/png",
          "sizes": "512x512"
        }
      ]
}
```

- After creating manfest file we need to link the manifest file with root page here in this case is index.html and also all those pages that need to be included in the PWA
```html
  <link rel="manifest" href="/manifest.json"/>

```
- After doing this we can go to chrome > application > manifest

- Need to host the application somewhere in order to download the app in home screen of a mobile
- For example we can host our pwa to netlify then we can go to the link and then the three dots of the chorme bar and click "ADD TO HOME SCREEN" it will download the PWA to our mobile phone


- In IOS some properties are not supported of manifest file




























