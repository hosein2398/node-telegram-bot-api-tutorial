
This is a beginners guide for [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api) .

 

 - Getting started with BotFather
 - Getting started
 -  Getting started
 
To create a bot on Telegram messenger firstly you need to contact with @BotFather .So go ahead and search for @BotFather in your messenger.
 Once you got there you should ask BotFather to give you a token.
 You could do this by typing  "/newbot" and sending it to BotFather.
Dear Father will ask you what you want to call you bot and you'll chose a name , and then you need to make a username for your bot , actually something that ends with the word 'bot' like: "my_test_bot".
If you write a username which is available, BotFather will send you a token.
Grab that token and keep somewhere safe.
![enter image description here](https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/BotFather.JPG)

Now that you bot is created maybe you want to set a description for that.
description are those messages showing in middle of the page usually  describing what this bot can do.
![enter image description here](https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/desc.JPG)

To set description for you bot in BotFather write "/setdescription" and send , then chose the bot you mean to change it's description and send description you want to be shown in you bot.


Ok now you'r ready to go. Create a node project and install bot-api:

    npm install --save node-telegram-bot-api
    
   Create a file index.js (or any other name) and inside the file require node-telegram-bot-api:
   
```js
    const TelegramBot = require('node-telegram-bot-api');
```
   Then you need to assign your token witch you got from BotFather:
   
```js
    const token = 'YOUR_TELEGRAM_BOT_TOKEN';
```
   And now create a new bot :
   
```js
    const bot = new TelegramBot(token, {polling: true});
```
  Lets try out our bot and do some real world things .We need to get messages that user sends us , to do so we would use following code:
  
```js
    bot.on('message', (msg) => {
    
     //anything
     
    });
```
Lets create simple greeting here. Here's big picture of our code :
```js
    const TelegramBot = require('node-telegram-bot-api'); 
    const token = 'YOUR_TELEGRAM_BOT_TOKEN';
    const bot = new TelegramBot(token, {polling: true});
    
    bot.on('message', (msg) => {
    
     //anything
     
    });
```
  We were try to greet and we'll do it here:
 
```js
    bot.on('message', (msg) => {
    
    var Hi = "hi";
    if (msg.text.toLowerCase().indexOf(Hi) === 0) {
    bot.sendMessage(msg.chat.id,"Hello dear user");
    } 
    
    });
```
Ok , now open up you command prompt and type:

    node index.js
  Go to your bot and hit on "/start" and then type "Hi" to it:
![enter image description here](https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/first%20message.JPG)

  So now that you know how to send and receive messages in your bot you may want to put some salt on it:
  
```js
    bot.on('message', (msg) => {

    var Hi = "hi";
    if (msg.text.toLowerCase().indexOf(Hi) === 0) {
    bot.sendMessage(msg.chat.id,"Hello dear user");
    } 
    
    var bye = "bye";
    if (msg.text.toLowerCase().includes(bye)) {
    bot.sendMessage(msg.chat.id, "Hope to see you around again , Bye");
    } 

    });
```
This time we're using "includes" method so if user sends us anything containing "bye" word we'll send him back the message:
![enter image description here](https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/Bye.JPG)
And definitely you can use any other string method that you want.

That's really common to send user a message describing use of bot while he taps on "/start". (these are called [commands](https://core.telegram.org/bots#commands))
To do so :
```js
    bot.onText(/\/start/, (msg) => {

    bot.sendMessage(msg.chat.id, "Welcome");
    
    });
  Lets create another command that will send a picture to user:
  

    bot.onText(/\/sendpic/, (msg) => {

    bot.sendPhoto(msg.chat.id,"https://www.somesite.com/image.jpg" );
    
    });
```
So now if you write "/sendpic" on your bot an image will be sent.
Sending audios is same and simple you can use "[sendAudio](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#TelegramBot+sendAudio)" method .

Now you might have seen some pictures containing caption with them like the following picture.
PIC
Well , How to to create these?
Answer is really simple you can send a caption with option on photo like so :
```js
    bot.onText(/\/sendpic/, (msg) => {

    bot.sendPhoto(msg.chat.id,"https://www.somesite.com/image.jpg",{caption : "Here we go ! \nThis is just a caption "} );
    
    });
```
  So now you know how to create captions and how to go to new line in your messages by typing \n .

Lets go a step further and start working with [keyboards](https://core.telegram.org/bots#keyboards).
keyboards are atcually the ones shown in this picture:
![enter image description here](https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/keyboard.jpg  =300x200)

Keyboards are nothing but an easy way to send messages. It's like your not forcing users to write something down and send it to bot but instead your demonstrating them some options that they can tap on and a message will be sent after that.
So lets see how we can create Keyboards , we'll send Keyboards on "/start" message:
```js
    bot.onText(/\/start/, (msg) => {
    
    bot.sendMessage(msg.chat.id, "Welcome", {
    "reply_markup": {
      "keyboard": [["Sample text", "Second sample"],   ["Keyboard"], ["I'm              robot"]]
        }
    });
    
    });
```
So now if you run you will see:
PIC

As I said in fact Keyboards are not nothing but automatic type and send for user. There is no difference if you write "I'm robot" and sending on your own or you click on Keyboard. Lets do something simple when that "I'm robot"  is received so add this up to your previous on message:
```js
    bot.on('message', (msg) => {
    var Hi = "hi";
    if (msg.text.toLowerCase().indexOf(Hi) === 0) {
        bot.sendMessage(msg.chat.id, "Hello dear user");
    }
    var bye = "bye";
    if (msg.text.toLowerCase().includes(bye)) {
        bot.sendMessage(msg.chat.id, "Hope to see you around again , Bye");
    }    
    var robot = "I'm robot";
    if (msg.text.indexOf(robot) === 0) {
        bot.sendMessage(msg.chat.id, "Yes I'm robot but not in that way!");
    }
    });
```
   So now if you go to your bot tap on start you see Keyboards and if you tap on I'm robot you'll see the message. Note that there is no difference if you type it or you send it by Keyboards.


node-telegram-bot-api does not have any method to get users information but in case if you want to you can get information like so:
```js
     var Hi = "hi";
    if (msg.text.toLowerCase().indexOf(Hi) === 0) {
       bot.sendMessage(msg.from.id, "Hello  " + msg.from.first_name);
    }
```
PIC
And if you wanted to get user profile pictures you can use [getUserProfilePhotos](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#telegrambotgetuserprofilephotosuserid-options--promise) .

More coming up soon.