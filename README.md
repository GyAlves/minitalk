# Minitalk

A simple communication program demonstrating inter-process communication using UNIX signals.

## 📖 Description

Minitalk is a project that implements a communication system between two programs using only UNIX signals `SIGUSR1` and `SIGUSR2`. The project consists of a server and a client where the client can send string messages to the server, and the server displays them upon receipt.

This project demonstrates:
- **Signal handling** in UNIX systems
- **Inter-process communication** (IPC)
- **Bit manipulation** for data transmission
- **Client-server architecture**

## ✨ Features

- **Signal-based communication**: Uses only `SIGUSR1` and `SIGUSR2` signals for data transmission
- **String transmission**: Send text messages from client to server
- **Process identification**: Server displays its PID for client connection
- **Error handling**: Robust signal handling and error management
- **Unicode support**: Handles extended ASCII and Unicode characters
- **Real-time display**: Server displays messages as they are received

## 🔧 How It Works

The communication protocol works as follows:

1. **Server startup**: The server starts and displays its Process ID (PID)
2. **Signal encoding**: Each character is transmitted bit by bit using two signals:
   - `SIGUSR1` represents bit `0`
   - `SIGUSR2` represents bit `1`
3. **Bit transmission**: The client sends 8 signals per character (for each bit)
4. **Message reconstruction**: The server receives signals, reconstructs bits into characters
5. **Display**: The server displays the complete message once transmission is finished

### Communication Flow

```
Client                          Server
  |                               |
  |  1. Get server PID           |
  |  2. Convert string to bits   |
  |  3. Send SIGUSR1/SIGUSR2 ----→ 4. Receive signals
  |     for each bit             |    5. Reconstruct bits
  |  6. Send termination signal --→    6. Form characters
  |                               |    7. Display message
```

## 🚀 Installation

### Prerequisites

- **Operating System**: Linux or macOS (UNIX-based system)
- **Compiler**: GCC or any C99-compatible compiler
- **Make**: Build automation tool

### Compilation

```bash
# Clone the repository
git clone https://github.com/GyAlves/minitalk.git
cd minitalk

# Compile the project
make

# This will create two executables:
# - server: The message receiver
# - client: The message sender
```

### Available Make Targets

```bash
make          # Compile both server and client
make server   # Compile only the server
make client   # Compile only the client
make clean    # Remove object files
make fclean   # Remove object files and executables
make re       # Recompile everything from scratch
```

## 📋 Usage

### 1. Start the Server

```bash
./server
```

The server will display its PID (Process ID):
```
Server PID: 12345
Waiting for messages...
```

### 2. Send Messages from Client

```bash
./client [SERVER_PID] [MESSAGE]
```

**Example:**
```bash
./client 12345 "Hello, World!"
```

**Output on server:**
```
Hello, World!
```

### Advanced Usage Examples

```bash
# Send a simple message
./client 12345 "Minitalk is working!"

# Send special characters
./client 12345 "Message with émojis: 🚀✨"

# Send numbers and symbols
./client 12345 "Testing: 123 @#$%^&*()"

# Send empty message (will send just a newline)
./client 12345 ""
```

## 📁 Project Structure

```
minitalk/
├── src/
│   ├── server.c          # Server implementation
│   ├── client.c          # Client implementation
│   └── utils.c           # Utility functions
├── include/
│   └── minitalk.h        # Header file with declarations
├── Makefile              # Build configuration
├── README.md             # Project documentation
└── LICENSE               # MIT License
```

## 🛠️ Technical Details

### Signal Handling

The project uses the following UNIX signals:
- **SIGUSR1**: Represents binary `0`
- **SIGUSR2**: Represents binary `1`

### Character Encoding

Each character is transmitted as 8 bits (1 byte):
- Characters are converted to their ASCII/Unicode values
- Each bit is sent as a separate signal
- Transmission order: Most Significant Bit (MSB) first

### Example Transmission

For the character 'A' (ASCII 65, binary 01000001):
```
Bit: 0 1 0 0 0 0 0 1
Signal: SIGUSR1 SIGUSR2 SIGUSR1 SIGUSR1 SIGUSR1 SIGUSR1 SIGUSR1 SIGUSR2
```

## ⚠️ Important Notes

### Limitations

- **Signal speed**: There's a small delay between signals to ensure reliable transmission
- **Process permissions**: Client and server must run with appropriate permissions
- **System limits**: Subject to system limits on signal delivery
- **Single client**: Server handles one client at a time

### Error Handling

The program handles various error conditions:
- Invalid PID provided to client
- Server process not found
- Signal delivery failures
- Invalid command-line arguments

## 🧪 Testing

### Basic Tests

```bash
# Test 1: Simple message
./server &
SERVER_PID=$!
./client $SERVER_PID "Test message"

# Test 2: Empty message
./client $SERVER_PID ""

# Test 3: Special characters
./client $SERVER_PID "émojis: 🎯🔥"

# Clean up
kill $SERVER_PID
```

### Error Cases

```bash
# Invalid PID
./client 99999 "This should fail"

# Missing arguments
./client

# Non-existent server
./client 1 "No server running"
```

## 🤝 Contributing

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **42 School** for the project specification and learning objectives
- **UNIX signal system** documentation and community resources
- **Contributors** who help improve this implementation

---

**Author**: [Gyasmin Alves](https://github.com/GyAlves)

**Project Link**: [https://github.com/GyAlves/minitalk](https://github.com/GyAlves/minitalk)