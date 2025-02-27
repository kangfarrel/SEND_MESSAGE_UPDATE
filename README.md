## Update sending message
I'm here to add some that are missing, and some new ones if use baileys felzar-Baileys.


#### Poll Result From Newsletter Message
```ts
await sock.sendMessage(
    jid,
    {
        pollResult: {
            name: "Text poll",
            votes: [["Options 1", 10], ["Options 2", 10]], // 10 For Fake Polling Count Results
        }
    }, { quoted : message }
)
```


#### Status Mentions 
```ts
await sock.sendStatusMentions(
     {
        text: "Hello", // or image / video / audio ( url or buffer )
     },
     [
      "123456789123456789@g.us",
      "123456789@s.whatsapp.net",
      // Enter jid chat here
     ] // If the Jid Group and Jid Private Chat are included in the JID list, try to make the JID group first starting from the Jid Private Chat or Jid Private Chat in the middle between the group Jid
)  
```


##### Cards Message
```ts
await sock.sendMessage(
    jid,
    {
        text: "Hello",
        footer: "Footer Message",
        cards: [
           {
              image: { url: 'https://example.jpg' }, // or buffer,
              title: 'Title Cards',
              caption: 'Caption Cards',
              footer: 'Footer Cards',
              buttons: [
                  {
                      name: "quick_reply",
                      buttonParamsJson: JSON.stringify({
                         display_text: "Display Button",
                         id: "ID"
                      })
                  },
                  {
                      name: "cta_url",
                      buttonParamsJson: JSON.stringify({
                         display_text: "Display Button",
                         url: "https://www.example.com"
                      })
                  }
              ]
           },
           {
              video: { url: 'https://example.mp4' }, // or buffer,
              title: 'Title Cards',
              caption: 'Caption Cards',
              footer: 'Footer Cards',
              buttons: [
                  {
                      name: "quick_reply",
                      buttonParamsJson: JSON.stringify({
                         display_text: "Display Button",
                         id: "ID"
                      })
                  },
                  {
                      name: "cta_url",
                      buttonParamsJson: JSON.stringify({
                         display_text: "Display Button",
                         url: "https://www.example.com"
                      })
                  }
              ]
           }
        ]
    },
    { quoted : message }
)
```


#### Album Message
```ts
await sock.sendAlbumMessage(
    jid,
    [
       {
          image: { url: "https://example.jpg" }, // or buffer
          caption: "Hello World",
       },
       {
          video: { url: "https://example.mp4" }, // or buffer
          caption: "Hello World",
       },
    ],
    { 
       quoted : message, 
       delay : 2000 // number in seconds
    }
)
```


#### Interactive Response Message
```ts
await sock.sendMessage(
    jid, 
    {
        buttonReply: {
             text: 'Text',
             nativeFlow: { 
                version: 3,
             },
        },
        type: 'interactive',
        ephemeral: true,
    }
)
```


#### Keep Message
- You need to pass the key of message, you can retrieve from [store](#implementing-a-data-store) or use a [key](https://baileys.whiskeysockets.io/types/WAMessageKey.html) object

- Time can be:

| Time  | Seconds        |
|-------|----------------|
| 24h    | 86.400        |
| 7d     | 604.800       |
| 30d    | 2.592.000     |

```ts
await sock.sendMessage(
    jid,
    {
        keep: message.key,
        type: 1, // 2 to unpin
        time: 86400    
    }
)
```


#### Pin Message
- You need to pass the key of message, you can retrieve from [store](#implementing-a-data-store) or use a [key](https://baileys.whiskeysockets.io/types/WAMessageKey.html) object

- Time can be:

| Time  | Seconds        |
|-------|----------------|
| 24h    | 86.400        |
| 7d     | 604.800       |
| 30d    | 2.592.000     |

```ts
await sock.sendMessage(
    jid,
    {
        pin: message.key
        type: 1, // 2 to unpin
        time: 86400        
    }
)
```


#### Group Invite Message With Thumbnail According to Jid
```ts
await sock.sendMessage(
    jid, 
    { 
        groupInvite: { 
            subject: "Your Group Name",  // Group name
            jid: "1234@g.us", // Group ID 
            text: "WhatsApp Group Invitation", // Additional information
            inviteCode: "CODE INVITATION", // Group invitation code
            inviteExpiration: 86400 * 3, // Expiration time in seconds (example: 86400 for 24 hours)
        } 
    },
    {
        quoted: message,
        getProfilePicUrl: sock.profilePictureUrl
    }
)
```


#### Group Invite Message With Thumbnail Custom
```ts
  const thumbnail = "https://example.jpg" // or buffer
// The image is used under 300 so that the thumbnail can be displayed 
  let Jimp = require("jimp");
  // import Jimp from "jimp"; => for type esm
  let img = await Jimp.read(thumbnail);
  let newWidth = img.bitmap.width;
  let newHeight = img.bitmap.height;
  if (newWidth > 300 || newHeight > 300) {
      const aspectRatio = newWidth / newHeight;
          if (aspectRatio > 1) {
             newWidth = 300;
             newHeight = Math.round(newWidth / aspectRatio);
          } else {
             newHeight = 300;
             newWidth = Math.round(newHeight * aspectRatio);
          }
      }
      let buff = await img
          .resize(newWidth, newHeight)
          .getBufferAsync(Jimp.MIME_JPEG);
          
await sock.sendMessage(
    jid, 
    { 
        groupInvite: { 
            subject: "Your Group Name",  // Group name
            jid: "1234@g.us", // Group ID 
            text: "WhatsApp Group Invitation", // Additional information
            inviteCode: "CODE INVITATION", // Group invitation code
            inviteExpiration: number, // Expiration time in seconds (example: 86400 for 24 hours),
            thumbnail: buff || null // if result not found or error
        } 
    },
    { quoted: message }
)
```


#### Request Payment Message Available To Quote Message
```ts
// Example non media sticker
await sock.sendMessage(
    jid,
    {
        requestPayment: {      
           currency: "IDR",
           amount: "10000000",
           from: "123456@s.whatsapp.net",
           note: "Hai Guys",
           background: { ...background of the message }
        }
    },
    { quoted : message }
)


// with media sticker buffer
await sock.sendMessage(
    jid,
    {
        requestPayment: {      
           currency: "IDR",
           amount: "10000000",
           from: "123456@s.whatsapp.net",
           sticker: Buffer,
           background: { ...background of the message }
        }
    },
    { quoted : message }
)


// with media sticker url
await sock.sendMessage(
    jid,
    {
        requestPayment: {      
           currency: "IDR",
           amount: "10000000",
           from: "123456@s.whatsapp.net",
           sticker: { url: Sticker Url },
           background: { ...background of the message }
        }
    },
    { quoted : message }
)
```


#### Event Message
```ts
await sock.sendMessage(
   jid, 
   { 
       event: {
           isCanceled: false, // or true for cancel event 
           name: "Name Event", 
           description: "Description Event",
           location: { 
               degressLatitude: -0, 
               degressLongitude: - 0 
           },
           link: Call Link,
           startTime: m.messageTimestamp.low,
           endTime: m.messageTimestamp.low + 86400, // 86400 is day in seconds
           extraGuestsAllowed: true // or false
       }
   },
   { quoted : message }
)
```


#### Poll Message
```ts
await sock.sendMessage(
    jid,
    {
        poll: {
            name: 'My Poll',
            values: ['Option 1', 'Option 2', ...],
            selectableCount: 1,
            toAnnouncementGroup: false // or true
        }
    },
    { quoted : message }
)
```


#### Interactive Message
```ts
// Example non header media
await sock.sendMessage(
    jid,
    {
        text: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        subtitle: "Subtitle Message",
        footer: "Footer Messages",
        interactiveButtons: [
             {
                name: "quick_reply",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     id: "ID"
                })
             },
             {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     url: "https://www.example.com"
                })
             }
        ]
    },
  {
    quoted : message
  }
)

// Example with media
await sock.sendMessage(
    jid,
    {
        image: { url : "https://example.jpg" }, // Can buffer
        caption: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        subtitle: "Subtile Message",
        footer: "Footer Messages",
        media: true,
        interactiveButtons: [
             {
                name: "quick_reply",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     id: "ID"
                })
             },
             {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     url: "https://www.example.com"
                })
             }
        ]
    },
  {
    quoted : message
  }
)

// Example with header product
await sock.sendMessage(
    jid,
    {
        product: {
            productImage: { url: "https://example.jpg }, //or buffer
            productImageCount: 1,
            title: "Title Product",
            description: "Description Product",
            priceAmount1000: 20000 * 1000,
            currencyCode: "IDR",
            retailerId: "Retail",
            url: "https://example.com",            
        },
        businessOwnerJid: "1234@s.whatsapp.net",
        caption: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        footer: "Footer Messages",
        media: true,
        interactiveButtons: [
             {
                name: "quick_reply",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     id: "ID"
                })
             },
             {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                     display_text: "Display Button",
                     url: "https://www.example.com"
                })
             }
        ]
    },
  {
    quoted : message
  }
)
```


#### Shop Message
```ts
// Example non header media
await sock.sendMessage(
    jid,
    {
        text: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        subtitle: "Subtitle Message",
        footer: "Footer Messages",
        viewOnce: true, // True it to be sent
        shop: "WA", // or number 3
        id: "Id Shop",
    },
  {
    quoted : message
  }
)

// Example with media
await sock.sendMessage(
    jid,
    {
        image: { url : "https://example.jpg" }, // Can buffer
        caption: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        subtitle: "Subtile Message",
        footer: "Footer Messages", 
        media: true,
        viewOnce: true, // True it to be sent               
        shop: "WA", // or number 3
        id: "Id Shop",
    },
  {
    quoted : message
  }
)

// Example with header product
await sock.sendMessage(
    jid,
    {
        product: {
            productImage: { url: "https://example.jpg }, //or buffer
            productImageCount: 1,
            title: "Title Product",
            description: "Description Product",
            priceAmount1000: 20000 * 1000,
            currencyCode: "IDR",
            retailerId: "Retail",
            url: "https://example.com",            
        },
        businessOwnerJid: "1234@s.whatsapp.net",
        caption: "Description Of Messages", //Additional information
        title: "Title Of Messages",
        footer: "Footer Messages", 
        media: true,
        viewOnce: true, // True it to be sent
        shop: "WA", // or number 3
        id: "Id Shop",
    },
  {
    quoted : message
  }
)
```



#### Payment Invite Message
```ts
await sock.sendMessage(
   jid, 
   {
      paymentInvite: {
         type: number of type,
         expiry: number expiry time of payment transaction
      },
   }
)
```


#### Order Message
```ts
await sock.sendMessage(
    jid,
    {
        order: {
           title: "Order Title",
           amount: Number of Amount * 1000,
           currency: "IDR", // Currency Code
           thumbnail: Thumbnail
           seller: "1234@s.whatsapp.net", // Seller Jid
           id: "id product",
           thumbnail: Buffer Or Url,
           itemCount: 9, 
           status: 1,
           surface: 1,
           text: Additional Information Of Order Message,
           token: "Token Order",
        }
    }
)
```


#### Invite Channel Admin
```ts
  const thumbnail = "https://example.jpg" // or buffer
// The image is used under 300 so that the thumbnail can be displayed 
  let Jimp = require("jimp");
  // import Jimp from "jimp"; => for type esm
  let img = await Jimp.read(thumbnail);
  let newWidth = img.bitmap.width;
  let newHeight = img.bitmap.height;
  if (newWidth > 300 || newHeight > 300) {
      const aspectRatio = newWidth / newHeight;
          if (aspectRatio > 1) {
             newWidth = 300;
             newHeight = Math.round(newWidth / aspectRatio);
          } else {
             newHeight = 300;
             newWidth = Math.round(newHeight * aspectRatio);
          }
      }
      let Buffer = await img
          .resize(newWidth, newHeight)
          .getBufferAsync(Jimp.MIME_JPEG);
await sock.sendMessage(
     jid, 
     {
         inviteAdmin: {
                 jid: "1234@newsletter", // Channel ID
                 subject: "WhatsApp", // Channel name        
                 text: "Accept this invitation to be an admin on my WhatsApp channel", // Additional information
                 inviteExpiration: number, // Expiration time in seconds (example: 86400 for 24 hours)
                 thumbnail: Buffer || null // Thumbnails in the form of an image buffer
         }
    }
)
```