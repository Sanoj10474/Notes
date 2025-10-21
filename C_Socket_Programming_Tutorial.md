# 🌐 C Socket Programming Tutorial

A complete beginner-friendly guide to writing **client-server communication programs in C** using sockets.

---

## 🧠 1. What Are Sockets?

Sockets allow two machines (or two programs) to communicate over a network — just like how your browser connects to a website.

- **Server**: Listens for connections.
- **Client**: Connects to the server.

Both sides use socket system calls to exchange data.

---

## ⚙️ 2. Required Header Files

Include these in every socket program:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
```

---

## 🖥️ 3. TCP Server Program

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

    // 3️⃣ Listen for connections
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

    // 5️⃣ Receive data
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

## 💻 4. TCP Client Program

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

## 🚀 5. How to Compile and Run

Open two terminals.

### 🖥️ Terminal 1 (Server):
```bash
gcc tcp_server.c -o server
./server
```

### 💻 Terminal 2 (Client):
```bash
gcc tcp_client.c -o client
./client
```

✅ Output:

**Server:**
```
Server listening on port 8080...
Message from client: Hello from Client!
Message sent to client
```

**Client:**
```
Message sent to server
Message from server: Hello from Server!
```

---

## 🧩 6. Next Steps

Once you master the basics, explore:
- Non-blocking sockets using `select()` or `poll()`
- UDP sockets (`SOCK_DGRAM`)
- Handling multiple clients with threads or `fork()`
- Using domain sockets for local IPC

---

## 🏁 Summary

| Concept | Function | Description |
|----------|-----------|-------------|
| Create Socket | `socket()` | Create an endpoint for communication |
| Bind | `bind()` | Assign address & port to socket |
| Listen | `listen()` | Wait for incoming connections |
| Accept | `accept()` | Accept a connection request |
| Connect | `connect()` | Client connects to the server |
| Send | `send()` | Send data |
| Receive | `recv()` | Receive data |
| Close | `close()` | Close the connection |

---

Made with ❤️ for C beginners learning networking!
