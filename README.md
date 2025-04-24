# Peril: A Risk-Inspired Strategy Game

Peril is a make-believe clone of the classic strategy game "Risk," designed to demonstrate the use of RabbitMQ and Docker in a distributed system. This project includes a client-server architecture where players interact with the game via commands, and the server manages the game state and logs.

## Features

- Multiplayer gameplay with commands for spawning armies, moving units, and checking game status.
- RabbitMQ for message-based communication between the client and server.
- Docker support for easy setup and deployment.

---

## Table of Contents

1. [Setup](#setup)
2. [How to Play](#how-to-play)
3. [Client Commands](#client-commands)
4. [Server Commands](#server-commands)
5. [Architecture](#architecture)

---

## Setup

### Prerequisites

- [Docker](https://www.docker.com/)
- [RabbitMQ](https://www.rabbitmq.com/)
- Go (for local development)

### Steps

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/phnthnhnm/peril.git
   cd peril
   ```

2. **Start RabbitMQ**:
   Use Docker to start a RabbitMQ container:
   ```bash
   docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
   ```
   Access the RabbitMQ management UI at `http://localhost:15672` (default credentials: `guest/guest`).

3. **Run the Server**:
   ```bash
   go run ./cmd/server/main.go
   ```

4. **Run the Client**:
   Open a new terminal and run:
   ```bash
   go run ./cmd/client/main.go
   ```

---

## How to Play

Peril is a turn-based strategy game where players compete to dominate the map by controlling territories and armies. Players interact with the game by entering commands in the client terminal.

### Objective

- Spawn armies, move them strategically, and conquer territories.
- Collaborate or declare war on other players to achieve dominance.

---

## Client Commands

The client supports the following commands:

| Command       | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| `spawn`       | Spawn new armies in your territories.                                       |
| `move`        | Move armies between territories.                                           |
| `status`      | Display the current game state, including your territories and armies.     |
| `help`        | Display a list of available commands.                                      |
| `spam <n>`    | Publish `<n>` malicious logs (for testing purposes).                       |
| `quit`        | Exit the game client.                                                     |

### Example Usage

- **Spawn Armies**:
  ```bash
  spawn
  ```
- **Move Armies**:
  ```bash
  move <from> <to> <number_of_units>
  ```
- **Check Status**:
  ```bash
  status
  ```

---

## Server Commands

The server supports the following commands:

| Command   | Description                                   |
|-----------|-----------------------------------------------|
| `pause`   | Pause the game for all players.              |
| `resume`  | Resume the game after a pause.               |
| `quit`    | Shut down the server.                        |

### Example Usage

- **Pause the Game**:
  ```bash
  pause
  ```
- **Resume the Game**:
  ```bash
  resume
  ```

---

## Architecture

### Client-Server Communication

- **RabbitMQ**: The client and server communicate using RabbitMQ for message passing.
  - **Exchanges**:
    - `ExchangePerilDirect`: Used for direct messages like pausing/resuming the game.
    - `ExchangePerilTopic`: Used for topic-based messages like army movements and logs.
  - **Queues**:
    - Transient queues for temporary messages (e.g., army moves).
    - Durable queues for persistent messages (e.g., game logs).

### Key Components

1. **Client**:
   - Handles user input and sends commands to the server via RabbitMQ.
   - Subscribes to game updates and logs.

2. **Server**:
   - Manages the game state and processes client commands.
   - Writes game logs and broadcasts updates.

---