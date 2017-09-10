Main concept explained
----------------------
Albot.Online works in a API kind of way, where it sends game information to your Bot and handles the actions that you respond with and then sends it further to a game server. We do this local communication using TCP or UDP messages depending on what game you are currently playing. 
The local game client will also display graphics for the chosen game, in hope of relieving you of the task of setting up a testing environment.
All game types also comes with a training mode that is very useful in the beginning of your Bot development. The training mode is basically just a “in house Bot” that you can use to make sure your development is going in the right direction.

# AI Bots
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

# Communicate with the server
The communication from your Bot is with your local Client, which in turn passes messages to and from the game server. All messages are formatted using regular ASCII text and are sent as TCP messages, and your Bot will need to keep an open socket connection during the whole game. We have code examples of of how to create a simple socket connection in C++, C#, and Python, but any other language of your choice can be used as well.

Currently all games are played using the Albot.Online game server, including single player training. In the future we hope to also provide a offline mode.

# Client
The Client works as an intermediary, and keeps a connection open to the game server as well as your local Bot. There are executables available for Windows, macOS, and Linux (untested).

### Connect to your client
Your Bot will run as a standalone application (run from your IDE or terminal window) alongside the Client. The steps to connect it are as follows: 
1. Start the Client application 
2. Navigate to the “Local Port” area in the game lobby. Located in the bottom right corner. Depicted in Figure 2.
3. Open port number X. This number can be any free port on your computer (take the default choice if you do not care)
4. Run your Bot from your IDE or terminal window, and make sure it connects to the same port as used in step 3. Make sure your Bot conforms to the communication protocol defined for the chosen game.
If everything ran correctly, you should see a “Connected” sign in the Client application

### Creating a new game
1. Navigate to the lobby.
2. Click the “Create game” button, this will take open the “Create Room” section.
3. Pick what type of game you want to create by selecting it in the “Game” section. See figure 3
4. Give your game room a name.
5. Click the “Create” button.
6. Once enough players have joined, the game starts automatically.

### Join an already created game:
1. Navigate to the lobby
2. Select an already existing game in the game list.
3. Click the “Join” button.
4. Once enough players have joined, the game starts automatically.
