# C Socket Programming Basics

## Table of Contents
1. [Introduction](#introduction)  
2. [What is a Socket?](#what-is-a-socket)  
3. [Client-Server Model](#client-server-model)  
4. [Basic Socket Functions](#basic-socket-functions)  
5. [Server Example](#server-example)  
6. [Client Example](#client-example)  
7. [Compiling and Running](#compiling-and-running)  
8. [References](#references)  

---

## Introduction
Socket programming allows communication between two computers over a network. In C, sockets are implemented using the `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, and `recv()` functions.  

---

## What is a Socket?
A **socket** is an endpoint for communication. Each socket is identified by:
- IP address
- Port number
- Protocol (TCP/UDP)

**Common types:**
- **TCP Socket:** Reliable, connection-oriented
- **UDP Socket:** Unreliable, connectionless

---

## Client-Server Model
- **Server:** Waits for incoming connections and responds
- **Client:** Initiates the connection and communicates with server

**Example:** Accessing a website
- Browser (Client) → Website Server  

---

## Basic Socket Functions

| Function | Purpose |
|----------|---------|
| `socket()` | Create a new socket |
| `bind()` | Bind a socket to an IP and port |
| `listen()` | Listen for incoming connections |
| `accept()` | Accept a client connection |
| `connect()` | Connect to a server |
| `send()` | Send data |
| `recv()` | Receive data |
| `close()` | Close the socket |

---

## Server Example

```c
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
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    char *message = "Hello from server";

    // Creating socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }

    // Bind
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY; // 127.0.0.1
    address.sin_port = htons(PORT);

    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }

    // Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }

    printf("Server listening on port %d\n", PORT);

    // Accept
    if ((new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen)) < 0) {
        perror("accept");
        exit(EXIT_FAILURE);
    }

    read(new_socket, buffer, BUFFER_SIZE);
    printf("Client: %s\n", buffer);
    send(new_socket, message, strlen(message), 0);
    printf("Message sent to client\n");

    close(new_socket);
    close(server_fd);
    return 0;
}
```

---

## Client Example

```c
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
    char *message = "Hello from client";
    char buffer[BUFFER_SIZE] = {0};

    // Creating socket
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        printf("\n Socket creation error \n");
        return -1;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);

    // Convert IPv4 addresses from text to binary form
    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        printf("\nInvalid address / Address not supported \n");
        return -1;
    }

    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        printf("\nConnection Failed \n");
        return -1;
    }

    send(sock, message, strlen(message), 0);
    printf("Message sent to server\n");
    read(sock, buffer, BUFFER_SIZE);
    printf("Server: %s\n", buffer);

    close(sock);
    return 0;
}
```

---

## Compiling and Running

1. **Compile Server:**
```bash
gcc server.c -o server
```

2. **Compile Client:**
```bash
gcc client.c -o client
```

3. **Run Server (first):**
```bash
./server
```

4. **Run Client:**
```bash
./client
```

---

## References
- [Beej’s Guide to Network Programming](http://beej.us/guide/bgnet/)
- [Linux man pages](https://man7.org/linux/man-pages/)

