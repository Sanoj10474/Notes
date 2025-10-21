# C Socket Programming Guide

A **socket program in C** enables communication between two systems over a network. It allows a client and server to exchange data using network protocols like TCP or UDP.

> In simple words: A socket acts as a virtual endpoint for data exchange between programs on different computers (or the same computer using `127.0.0.1`).

---

## 1️⃣ Creating a Socket
```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
```
- `AF_INET` → Use IPv4
- `SOCK_STREAM` → TCP socket (reliable, connection-based)
- `0` → Protocol (default TCP for stream sockets)

`sockfd` acts as a handle through which your program sends or receives data.

---

## 2️⃣ How Communication Works

### Server Side
1. Creates a socket
2. Binds it to an IP + port (like assigning a phone number)
3. Listens for incoming calls (`listen()`)
4. Accepts a client’s connection (`accept()`)
5. Communicates using `send()` and `recv()`

### Client Side
1. Creates a socket
2. Connects to the server (`connect()`)
3. Sends and receives data

---

## 3️⃣ Types of Sockets
| Socket Type | Description | Example |
|-------------|------------|--------|
| SOCK_STREAM | TCP Socket – connection-based, reliable | Web browser, SSH |
| SOCK_DGRAM  | UDP Socket – connectionless, faster, less reliable | Video streaming, DNS |
| SOCK_RAW    | Raw Socket – used for low-level protocols | Packet sniffers, custom protocols |

**Example:**
- Open a browser → Browser creates a socket → Connects to server IP & port 80 → Sends HTTP request → Receives response → Closes socket.

---

## 4️⃣ Required Header Files
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>
```

| Header       | Purpose |
|--------------|--------|
| stdio.h      | Input/output functions (`printf`, `scanf`) |
| stdlib.h     | Utilities (`exit`, `malloc`, `free`) |
| string.h     | String manipulation (`strcpy`, `memset`, `strlen`) |
| unistd.h     | Close sockets, read/write from file descriptors |
| sys/socket.h | Core socket functions: `socket()`, `bind()`, `listen()`, `accept()`, `connect()` |
| arpa/inet.h  | IP address conversions: `htons()`, `inet_pton()`, `inet_addr()` |

---

## 5️⃣ Socket Workflow

### Server
```
socket() → bind() → listen() → accept() → recv()/send() → close()
```

### Client
```
socket() → connect() → send()/recv() → close()
```

---

## 6️⃣ Key Socket Functions
| Function | Prototype | Purpose |
|----------|-----------|--------|
| socket() | int socket(int domain, int type, int protocol) | Create a socket and return file descriptor |
| bind()   | int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen) | Assign IP & port to socket (server only) |
| listen() | int listen(int sockfd, int backlog) | Mark socket ready to accept connections (server only) |
| accept() | int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen) | Accept a client connection |
| connect()| int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen) | Connect client to server |
| send()   | ssize_t send(int sockfd, const void *buf, size_t len, int flags) | Send data |
| recv()   | ssize_t recv(int sockfd, void *buf, size_t len, int flags) | Receive data |
| read()   | ssize_t read(int fd, void *buf, size_t count) | Receive data from socket |
| write()  | ssize_t write(int fd, const void *buf, size_t count) | Send data through socket |
| close()  | int close(int fd) | Close socket |
| htons()  | uint16_t htons(uint16_t hostshort) | Host to network byte order (port) |
| htonl()  | uint32_t htonl(uint32_t hostlong) | Host to network byte order (IP) |
| ntohs()  | uint16_t ntohs(uint16_t netshort) | Network to host byte order (port) |
| ntohl()  | uint32_t ntohl(uint32_t netlong) | Network to host byte order (IP) |
| inet_pton() | int inet_pton(int af, const char *src, void *dst) | IP string to binary |
| inet_ntop() | const char *inet_ntop(int af, const void *src, char *dst, socklen_t size) | Binary IP to string |

---

## 7️⃣ Quick Example
```c
// Create TCP socket
int sockfd = socket(AF_INET, SOCK_STREAM, 0);

// Bind server to port 8080
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(8080);
addr.sin_addr.s_addr = INADDR_ANY;
bind(sockfd, (struct sockaddr*)&addr, sizeof(addr));

// Listen for clients
listen(sockfd, 5);

// Accept a client
int client_sock = accept(sockfd, NULL, NULL);

// Send/Receive data
char msg[] = "Hello Client";
send(client_sock, msg, strlen(msg), 0);
char buffer[1024];
recv(client_sock, buffer, sizeof(buffer), 0);

// Close sockets
close(client_sock);
close(sockfd);
```

---

## 8️⃣ Byte Order (Endianness)
- Network protocols use **big-endian**.
- Functions:
  - `htons()` → Host to network short (port)
  - `htonl()` → Host to network long (IP)
  - `ntohs()` / `ntohl()` → Convert back to host order

Always use these when sending port numbers or IP addresses.

---

## 9️⃣ IP Addresses and Ports
- **IP address** – Identifies the machine (like a home address)
- **Port number** – Identifies the application/service on that machine (like a room number)

**Common Ports:**
- HTTP → 80
- HTTPS → 443
- FTP → 21

---

## 10️⃣ Step-by-Step TCP Server
```c
// tcp_server.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    char buffer[BUFFER_SIZE] = {0};
    char *message = "Hello from Server!";
    int addrlen = sizeof(address);

    // 1️⃣ Create socket
    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (server_fd == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }

    // 2️⃣ Bind to port
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);

    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }

    // 3️⃣ Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }

    printf("Server listening on port %d...\n", PORT);

    // 4️⃣ Accept connection
    new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t *)&addrlen);
    if (new_socket < 0) {
        perror("accept");
        exit(EXIT_FAILURE);
    }

    // 5️⃣ Receive message
    read(new_socket, buffer, BUFFER_SIZE);
    printf("Message from client: %s\n", buffer);

    // 6️⃣ Send response
    send(new_socket, message, strlen(message), 0);
    printf("Message sent to client\n");

    close(new_socket);
    close(server_fd);
    return 0;
}
```

---

## 11️⃣ Step-by-Step TCP Client
```c
// tcp_client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[BUFFER_SIZE] = {0};
    char *message = "Hello from Client!";

    // 1️⃣ Create socket
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        printf("Socket creation error\n");
        return -1;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);

    // 2️⃣ Convert IPv4 address
    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        printf("Invalid address\n");
        return -1;
    }

    // 3️⃣ Connect to server
    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        printf("Connection Failed\n");
        return -1;
    }

    // 4️⃣ Send and receive message
    send(sock, message, strlen(message), 0);
    printf("Message sent to server\n");

    read(sock, buffer, BUFFER_SIZE);
    printf("Message from server: %s\n", buffer);

    close(sock);
    return 0;
}
```

---

## 12️⃣ How to Run
```bash
gcc tcp_server.c -o server
gcc tcp_client.c -o client

# In separate terminals
./server
./client
```

**Expected Output:**

**Server Terminal:**
```
Server listening on port 8080...
Message from client: Hello from Client!
Message sent to client
```

**Client Terminal:**
```
Message sent to server
Message from server: Hello from Server!
```

---

**End of C Socket Programming Guide**

