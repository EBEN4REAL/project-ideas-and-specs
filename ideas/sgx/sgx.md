# Story game specification document

## Overview

The idea for this game is to deploy a two-player, text-based game in which users take turn in writing paragraphs of a story and spits out the story all formatted and shii.

This specification is my attempt to document how the story game should work in general. From game flow, to more technical specifications like software stack etc.

I will attempt to take a deep-dive, using the [12factor app spec](12factor.net) as a guide.

Implementation can be found [here](https://github.com/OpeOnikute/sgx).

## Game flow

**_Player one_** - Session creator.
**_Player two_** - Session guest/joinee.

- Player one starts the game. Enters title of story and his/her name.
- Player two joins the game.
- Each player gets one minute un-restricted to type in a paragraph.
- If a player hasn't entered non-empty text after the full minute, the other player gets his own next minute.
- When players are done, player one can decide to end.
- The title is chosen at the end.
- When the game ends, the system provides box that contains the entire story, formatted in paragraphs.
- Player one can then:
  - edit the content of that box
  - copy the text out
  - share a link to the story (on Twitter and Facebook)
  - print out as pdf (and Word?)

**Meta**

- Title can be edited throughout the session.

## Components

2 main components - API and Frontend.

Components are seperated on a code level, not on an infrastructure level.

### Frontend

UI inspired by [Fitness app chatbot ui ux
by Virgil Pana](https://dribbble.com/shots/3850121-Fitness-app-chatbot-ui-ux).

**Behaviour**

- Provides the interface for users to play.
- Assigns a user a random animal image when they join.
- Regulates the session and decides whose turn it is to play.
- Only switches turns if the player is not currently typing.
- Concatenates the story on each user entry and sends updates to the API in 3-minute intervals.
- Sends final update when the game ends.

**Stack**

- React

**Screens**

- New story

<img width="720px" src="assets/create-story.png" alt="creat story">

<img width="720px" src="assets/create-story-modal.png" alt="create story modal">

- Join story

<img width="720px" src="assets/join-story.png" alt="join story">

- Collaborate 

<img width="720px" src="assets/story-page.png" alt="collab">

- View story (final screen)

<img width="720px" src="assets/view-story.png" alt="view story">

### API

**Behaviour**

- Handles authentication (if any)
- Database reads & writes
- Exposes data through REST endpoints.
- Storing each entry with meta data of who wrote it.
- Uses player IDs, not names, for identification.

**Stack**

- NodeJS with Express or Go

**Endpoints**

- Create story session (POST) - Player one name & email.
- Join session (PUT) - `Player two name`
- Add new paragraph - `Player id & text`
- Save Story (PUT)
- View Story (GET)

**Story Data Structure Example**

```
list = [
    {"user": "<player1-id>", "text": "There was a man with big balls."},
    {"user": "<player2-id>", "text": "He spent time making them dance."},
    {"user": "<player1-id>", "text": "He was eventually arrested for sexual assault."}
]

//Output
There was a man with big balls.
He spent time making them dance.
He was eventually arrested for sexual assault.

Story by: <player1-name> & <player2-name>
```

### External Services

- **Real-time connection** - Socket.io or PubNub.

### Action Points

- Select a better name.
