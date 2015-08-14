# ios-components-bikeshedding
often we create same components again and again. this is short reminder that simple feature contains really a lot of pieces

--------------

## Chat

### Chat UI 
You can select from different libs, with or without bubbles:

  * https://github.com/jessesquires/JSQMessagesViewController (huge library with many customization options, but release 7 is real mess inside. controller incapsulates datasource, delegate and view logic)
  * https://github.com/slackhq/SlackTextViewController
  * https://github.com/LoganWright/SimpleChat

### Basic actions

As user i want to:

  * send text message (image, my current location, video)
  * receive message
  * read history (older messages)
  * fetch new messages (newer that user has; user may miss these messages on laggy connection)

### Receiving messages
  
  * manually reload chat to fetch new messages
  * fetch new messages automatically
    * use HTTP polling (easy way) 
    * use web sockets
  * show that user is typing message 
  * scroll to new message

### Sending messages
  
  * show typing indicator for one user when another starts typing
  * send message

### Message status

  * message is being typed
  * message was sent
  * message was delivered
  * message was read/open
  * message failed to send (read below)

### Connection issues

  * message sending error: 
    * indicate that message sending failed (show warning button/icon, change message background)
    * tap on message -> show retry button/action sheet
    * re-send message
    * put message in correct place (handle sending order)
  
  * message receiving error (no connection or server error):
    * show 'establishing connection' indicator
    * fetch new messages when connection was established

### Message order
  
User should see messages in order as they were send. Racecondition could appear due to connection lags. Set sequence number of timestamp on client side to sort messages in correct order. 

### Empty view

Show cute 'no messages' view to push user to start chatting.
Lib https://github.com/dzenbot/DZNEmptyDataSet

### Notifications
  
 * send push notification when new message is received
 * receive push notification and open correspondent chat window
 * allow to hide content/author of message (a-la Messages)
 * virbate/play audio when message is received/sent
  

### Unread
 
 * show number of unread messages
 * indicate that chat has unread messages in list of chats
 * show 'unread' messages inside chat
 * mark message as read

### Transport layer

You can send messages via http to your server or user sockets/mqtt.

 * ios side:
   * https://github.com/square/SocketRocket
   * https://github.com/jmesnil/MQTTKit
 * server side + ios lib:
   * https://pusher.com/
   * https://www.pubnub.com/
   * https://github.com/RocketChat/Rocket.Chat

### Security

Use https and encrypt messages using private keys.

Encryption libs:

  * https://github.com/cossacklabs/themis/
  * https://github.com/RNCryptor/RNCryptor
  * https://github.com/aerogear/aerogear-crypto-ios


### Application life circle

Active chat always shows actual data: last chat message, message counter, chat name, user's name.

Ensure handling correctly such cases:

 * chat is opened > device looses connection > device restores connection
 * chat is opened > connection type changes from wi-fi to cellular and vice versa
 * chat is opened > device screen is turned off manually > user activates device and app opens
 * chat is opened > device goes to sleep > user activates device and app opens
 * chat is opened > app goes to background > app goes to foreground

### Media

  * show placeholder while media data is downloading
  * allow to cancel sending message (stop upload media file)
  * save photos/videos to camera roll

### Advanced
  
 actions:
  * allow to edit text message
  * allow to delete message (text, media). show delete animation
  * share message (open native sharing action sheet)
  
 logic:
  * store local history (when user opens chat, show cached messages)
  * reload chat in background to show always latest data
  * allow to rename chat
  * open user profile on tapping on his avatar
 
 ui:
  * custom keyboard/stickers
  * message bubbles dynamic behaviour (if any)
  * data types support: urls, tel, dates (tap on link - open webpage (in Safari, in inner webbrowser))
