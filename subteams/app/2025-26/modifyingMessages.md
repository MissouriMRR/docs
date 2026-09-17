# Creating a New Message

1. Add the message type to the `MessageTypes` class in `networking.dart`
2. To send the message, call:
    ```dart
    sendData(
        Data(
            MessageTypes.<New_Message>,
            [1],
            { <additional json data> }
        ).toJson()
    )
    ```
3. To receive the message, add it to the switch case in `handleServerMessage` (also in `networking.dart`)
