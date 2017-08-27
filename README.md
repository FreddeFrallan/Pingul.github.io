Albot.Online User Guide
=======================

Main concept explained
----------------------
Albot.Online works in a API kind of way, where it sends game information to your Bot and handles the actions that you respond with and then sends it further to a game server. We do this local communication using TCP or UDP messages depending on what game you are currently playing. 
The local game client will also display graphics for the chosen game, in hope of relieving you of the task of setting up a testing environment.
All game types also comes with a training mode that is very useful in the beginning of your Bot development. The training mode is basically just a “in house Bot” that you can use to make sure your development is going in the right direction.

AI Bots
----------------------
### What is a Bot?
A Bot is a program running locally on your computer that will be used to send game commands. You will need to create a distinct Bot for each game you play. The typical pseudo-code for the main loop of a Bot looks like:

```
connectToPort(4000) // same port as in Client, see following
forever do       // sections
    msg <- waitForTCPMessage()
    if msg is “game over” then 
        exit()
    gameState <- parseMessage(msg)
    action <- nextAction(gameState)
    sendTCPMessage(parseAction(action))
```

The important function is the `nextAction(gameState)`, which is unique for every game and the core of your Bot. Here it is totally up to you how your Bot will process the current position and try to calculate the best move. We impose no rules whatsoever—--you could even mix your own user input together with your Bot’s own calculations.

Communicate with the server
----------------------
The communication from your Bot is with your local Client, which in turn passes messages to and from the game server. All messages are formatted using regular ASCII text and are sent as TCP messages, and your Bot will need to keep an open socket connection during the whole game. We have code examples of of how to create a simple socket connection in C++, C#, and Python, but any other language of your choice can be used as well.

Currently all games are played using the Albot.Online game server, including single player training. In the future we hope to also provide a offline mode.

Figure 1: Simplified schematic of how different players are connected.

Client
----------------------
The Client works as an intermediary, and keeps a connection open to the game server as well as your local Bot. There are executables available for Windows, macOS, and Linux (untested).

### Connect to your client
Your Bot will run as a standalone application (run from your IDE or terminal window) alongside the Client. The steps to connect it are as follows: 
1. Start the Client application 
2. Navigate to the “Local Port” area in the game lobby. Located in the bottom right corner. Depicted in Figure 2.
3. Open port number X. This number can be any free port on your computer (take the default choice if you do not care)
4. Run your Bot from your IDE or terminal window, and make sure it connects to the same port as used in step 3. Make sure your Bot conforms to the communication protocol defined for the chosen game.
If everything ran correctly, you should see a “Connected” sign in the Client application

Figure 2: Local Port area being marked in yellow.

### Creating a new game
1. Navigate to the lobby.
2. Click the “Create game” button, this will take open the “Create Room” section.
3. Pick what type of game you want to create by selecting it in the “Game” section. See figure 3
4. Give your game room a name.
5. Click the “Create” button.
6. Once enough players have joined, the game starts automatically.

Figure 3: Create room section.

### Join an already created game:
1. Navigate to the lobby
2. Select an already existing game in the game list.
3. Click the “Join” button.
4. Once enough players have joined, the game starts automatically.

Games
-------------------------------
### Connect 4
Game play can be seen at https://en.wikipedia.org/wiki/Connect_Four.
#### Receive protocol
These messages must be handled by the Bot:
Name
Type
Description
Move request
string
42 integers separated by space ( ) defining the current board position. After “Board position” has been received, the Bot has to send a “Next move” command.
Game over
string
Duh.

##### Message: Move request
This message will be sent to your local Bot containing the current board position. The board position is represented by a 6 by 7 matrix where every cell has one of the following states:
- 1  = Your playing piece
- 0  = Empty square
- -1 = Opponent playing piece
Note: This means that whether you are currently playing as Red or Yellow, your pieces will always be 1 and your opponents -1. 

The board matrix will be sent to you in a plain text message containing 42 numbers all separated by one space. These numbers will be written column by column as you can see in figure 4. See figure 5 for an example message.

Figure 4: Each number indicates the index position in the list of values that are sent.

Figure 5: Example board. If you were playing as the yellow pieces and you received a move request containing the above position, the message would look like:
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 -1 -1 0 0 1 -1 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0

##### Message: Game over
Self explanatory. Contains the exact message “GameOver”, and indicates that the game is over.

#### Send protocol
These messages must be sent by the Bot:
Name
Type
Description
Player move
string
One (1) number between 0-6 indicating the next move. Sent after a Move request message has been received. Note: Formatted as a string.
Message: Player move
This is the message that you send containing information about what move you wish to make. You simply send a text message referring to what column you chose to drop your next piece in. The columns are zero indexed, meaning you are expected to send a message from 0-6.
Depicted in this picture to the right.

If Albot.Online for whatever reason does not accept your move it will resend the move request until you give a proper response.


