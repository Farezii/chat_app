# chat_app

Flutter project for a messaging app, includes Firebase implementation, Firebase authorization and account creation, database manipulation, notification and permission messages, as well as native functions such as camera photos.

## Getting Started

This project uses the flutter-chat-app Firebase project. It is currently fully functional.

Firstly, after installing flutter:
- Install Node.js from the [official installer](https://www.nodejs.org/).
- Using npm, run `npm install -g firebase-tools`
- Enter the Firebase CLI using the Command Prompt (NOT Powershell).
- `firebase login`
- In the flutter project directory, install `dart pub global activate flutterfire_cli`
- Within the Firebase CLI in CMD, run `flutterfire configure`

Once installed, you can connect to a Firebase project of choice and utilize the [many plugins](https://firebase.google.com/docs/flutter/setup?platform=ios#available-plugins) to fit your needs.

## Project file overview

# Screens

- `main.dart`: Main App file, from which the project is run. Contains the Firebase initialization, base Theme and a StreamBuilder object to detect wether the user is logged in or not, and which screen to show. Shows loading circle while waiting for a response from the server.

- `splash.dart`: Screen made only to show a Circular Progress Indicator while the app connects to Firebase.

-`screens/auth.dart`: Authorization screen and logic using Firebase methods. The user has the choice of creating an account in case he does not have it, in which he has to provide:
    - E-mail
    - Public Username to be used in the app
    - Password longer than 6 characters
    - Photo taken from the camera.
The information and photo given are then validated and registered within Firebase with their email and password, the photo is stored in the cloud, and a new database entry for collection `users` is created, using the user's UID as the main key, their username, email and URL of the image.
If already registered, user can use their account to connect to the Firebase project and enter the app.

- `chat.dart`: Basic file containing a function that allows notifications from the `chat` topic, and a basic overlay showing the `NewMessage()` widget, topped by a stack of `ChatMessages()` widgets.

# Widgets

- `user_image_picker.dart`: StatefulWidget that utilizes ImagePicker to obtain a photo from the device's camera.

- `new_message.dart`: StatefulWidget containing a single `TextField()` widget in which the user writes a message in, and afterwards send via the button to the right. Once tapped, the widget takes the message via `_messageController` and sends the following information to the `chat` collection:
    - `text`: Written message;
    - `createdAt`: Timestamp of creation of the message;
    - `userId`: UID from the current user;
    - `username`: Username from the current user;
    - `userImage`: Image URL from the current user's avatar, saved in Firebase;

- `chat_messages.dart`: StatelessWidget that shows messages sent to the app by all users, in descending order of creation. Utilizes `message_bubble.dart` to properly theme and organize the bubbles according to the owner of the current message and the onwer of the next message.

- `message_bubble.dart`: StatelessWidget containing `MessageBubble.first` and `MessageBubble.next` enums, in order to better organize the chat depending on consecutive messages by the same user or different messages from multiple users. Then builds the bubble using the given information:
    - `username`: String containing the username of the user. If not the first message in the sequence, this value is `NULL`;
    - `userImage`: String containing the URL for the avatar of the user. If not the first message in the sequence, this value is `NULL`;
    - `message`: Non-nullable String containing the written message by the user;
    - `isMe`: Boolean to check if the previous message was from the same user or not;
    - `isFirstInSequence`: Boolean to tell wether or not this is the first message of the sequence for this user. The `first` enum sets this to true, while `next` sets it to false;



