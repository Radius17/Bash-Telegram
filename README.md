## Bash-Telegram

These scripts are used to send Telegram messages from a Debian or Ubuntu server.

#### Create your Telegram Bot
Create your own Bot with the help of @BotFather. 

[This page](https://core.telegram.org/bots) explain very clearly what you can expect from your Telegram Bot and how to create it.

Once you have created your Bot, you get your API key. This key looks like:
> 123456789:AABBCCDD_abcdefghijklmnopqrstuvwxyz.

Your Bot will be able to send messages either to :

- your personnal account
- any channel it belongs to

To send any message, your Bot will need to use either your user ID or a Channel ID. 

Here is the way to get these IDs.

#### Get your User ID
To get your user ID, you just need to :

- send a message to your Bot from your Telegram client
- call the following URL from any web browser

> **URL:**
`https://api.telegram.org/bot123456789:AABBCCDD_abcdefghijklmnopqrstuvwxyz/getUpdates`

> **Answer:** 
{"ok":true,"result":[{"update_id":563273027,"message":{"message_id":505, "from":"id":11223344, "first_name":"YourFirstName", "last_name":"YourLastName", "username":"YourUserName"},
"chat":{"id":11223344, "first_name":"YourFirstName", "last_name":"YourLastName", "username":"YourUserName", "type":"private"}, "date":1483150094, "text":"Hello bot !"}}]}

You can see here that your user ID is `11223344`. This is the client ID that Telegram Bot should use to send you a message.

#### Get Channel ID
If your server must notify multiple users, it should notify a channel.

First, from your Telegram client, create a channel and keep it public as you need to obtain the channel ID. Give a name @YourNewChannelName to your channel.

Next, from your Bot thread, go to the Bot parameters page. You should see that your Bot is in both Members and Administrators list.

The Bot is now ready to publish messages.

To get your channel ID, you just need to send a message to `@YourNewChannelName` from any web browser using your Bot API key :

> **URL:**
`https://api.telegram.org/bot123456789:AABBCCDD_abcdefghijklmnopqrstuvwxyz/sendMessage?chat_id=@YourNewChannelName&text=FirstMessage`

> **Answer:**	
{"ok":true, "result":{"message_id":2, "chat":{"id":-1234567890123, "title":"Your Channel", "username":"YourNewChannelName", "type":"channel"}, "date":1483383169, "text":"FirstMessage"}}

You can see in the resulting JSON string that your new channel ID is `-1234567890123`.

You can now edit your Channel and convert its type to Private.

Your Bot can still send a message to the channel using the Channel ID :

> **URL:** 
`https://api.telegram.org/bot123456789:AABBCCDD_abcdefghijklmnopqrstuvwxyz/sendMessage?chat_id=-1234567890123&text=SecondMessage`

> **Answer:**
{"ok":true, "result":{"message_id":2, "chat":{"id":-1234567890123, "title":"Your Channel", "username":"YourNewChannelName", "type":"channel"}, "date":1483383169, "text":"SecondMessage"}}

#### Functionalities

Main functionality of Telegram notification script is to send a Telegram notification. It allows to :

- display plain text message or a file content (log etc.)
- display a title
- display an emoji as leading icon  (codes may be found in this emoji table)
- add a photo or a document
- select formatting style as Markdown (default) or HTML
- disable message notification on the client side
- select Bot to use to send the message (thru API key)
- set message recipient ID


#### Check Locale Settings
You have to make sure that your locale is set to UTF-8 on your server to properly encode icons.

```sh
# locale
LANG=ru_RU.UTF-8
LC_CTYPE="ru_RU.UTF-8"
LC_NUMERIC="ru_RU.UTF-8"
```

If not, you should set locale to UTF-8 according to your langage :

```sh
# update-locale LANG=ru_RU.UTF-8 LANGUAGE=ru.UTF-8
# reboot
```

#### Script configuration
Telegram notification script is using 2 default parameters that should be configured in 
> /etc/telegram-notify.conf

```sh
[general]
api-key=YourAPIKey
user-id=YourUserOrChannelID

[network]
socks-proxy=
```
When calling the notification script these default parameters may be surcharged with --user and --key parameters.

For custom config file use:
>  --config path_to_file/telegram-notify.conf

### Examples

```sh
telegram-notify --success --text "Action *sucessful* with markdown *bold* example"
```
```sh
telegram-notify --error --title "Error" --text "Error message with a title"
```
```sh
telegram-notify --question --title "File content display" --text "/tmp/log.txt"
```
```sh
telegram-notify --icon 1F355 --text "Message with custom icon 1F355 and embedded image" --photo "/tmp/icon.png"
```
```sh
telegram-notify --text "Result is available in the embedded document" --document "/tmp/result.log"
```

