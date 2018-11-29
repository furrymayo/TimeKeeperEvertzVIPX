# TimeKeeper
A Node.js server supporting a generic API to create and display timers, countdowns, and messages to client browsers.

## Installing this software:
Copy the code to a working directly on your computer. Make sure Node is installed.
Run `npm install` to make sure all required modules are downloaded and installed.
Run `node main.js` to start the server.

## Using the API:
### Rooms:
Rooms allow you to control and specify which timers appear. This is helpful if you are running clocks in multiple venues or instances, and only want certain timers to appear on certain viewer screens.

Getting All Rooms:
Make a GET request to `/api/rooms` to receive a JSON reply of all available rooms.
Room objects are defined as:

* `id`: Unique ID generated by the server upon creation of the Room object.
* `name`: The friendly name of the room, displayed to the user when connecting to the viewer client.
* `enabled`: Boolean true/false, indicates if the Room is available for selection or not.

Getting a specific Room:
Make a GET request to `/api/room/roomID`, where *roomID* is the ID of the room, to receive a JSON reply of the room information.
If an invalid Room ID is specified, a reply of `{returnStatus: "invalid-room-id"}` will be returned. 

Adding a Room:
Make a POST request to `/api/room/add`, specifying all the Room object data, except for the ID, which will be automatically generated. The newly created Room object will be returned as a JSON reply. 

Updating a Room:
Make a POST request to `/api/room/roomID`, where roomID is the ID of the Room object, along with a JSON Room object in the POST body. It will return the updated Room object as a JSON reply. 

Deleting a Room:
Make a POST request to `/api/room/delete/roomID`, where *roomID* is the ID of the Room object. 

### Timers:
Timers are the objects that TimeKeeper will count down to, based on the viewer's current local system time.

Getting All Timers:
Make a GET request to `/api/timers` to receive a JSON reply of all available timers.

Getting a specific Timer:
Make a GET request to `/api/timer/timerID`, where timerID is the ID of the timer, to receive a JSON reply of the room information.
If an invalid Timer ID is specified, a reply of {returnStatus: "invalid-timer-id"} will be returned. 
Timer objects are defined as:

* `id`: Unique ID generated by the server upon creation of the Timer object.
* `datetime`: The datetime in epoch milliseconds that the viewer page should count down to. See JavaScript date object and the getTime() method for more information.
* `label`: Friendly label to display alongside the timer, for a description.
* `expireMillis`: The number of milliseconds left in the countdown where the timer should start blinking red.
* `publishMillis`: The number of milliseconds left in the countdown where the timer should appear on the viewer page.
* `roomID`: The Room ID that this Timer should appear on. Specifying "room-0" means this Timer will be sent to all viewer pages regardless of the room they are viewing.

Adding a Timer:
Make a POST request to `/api/timer/add`, specifying all the Timer object data, except for the ID, which will be automatically generated. The newly created Timer object will be returned as a JSON reply. 

Updating a Timer:
Make a POST request to `/api/timer/timerID`, where *timerID* is the ID of the Timer object, along with a JSON Timer object in the POST body. It will return the updated Timer object as a JSON reply. 

Deleting a Timer:
Expired Timers older than 5 minutes are automatically deleted, however if you wish to manually delete a timer, make a POST request to `/api/timer/delete/timerID`, where *timerID* is the ID of the Timer object. 

### Messages:
Messages can be sent and displayed on viewer clients.

Getting All Messages:
Make a GET request to `/api/messages` to receive a JSON reply of all available Message objects.

Message objects are defined as:

* `id`: Unique ID generated by the server upon creation of the Message object.
* `datetime`: The datetime in epoch milliseconds that the viewer page should count down to. See JavaScript date object and the getTime() method for more information.
* `message`: The actual message to display. HTML is OK to use.
* `expireMillis`: The number of milliseconds left in the countdown where the Message should start blinking red.
* `publishMillis"`: The number of milliseconds left in the countdown where the Message should appear on the viewer page.
* `roomID`: The Room ID that this Timer should appear on. Specifying "room-0" means this Message will be sent to all viewer pages regardless of the room they are viewing.

Adding a Message:
Make a POST request to `/api/message/add`, specifying all the Message object data, except for the ID, which will be automatically generated. The newly created Message object will be returned as a JSON reply. 

Updating a Message:
Make a POST request to `/api/message/messageID`, where *messageID* is the ID of the Message object, along with a JSON Timer object in the POST body. It will return the updated Timer object as a JSON reply. 

Deleting a Message
Expired Messages older than 5 minutes are automatically deleted, however if you wish to manually delete a timer, make a POST request to `/api/message/delete/*messageID*`, where *messageID* is the ID of the Message object. 