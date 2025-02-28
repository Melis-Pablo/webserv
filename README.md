# Webserv

## 🌐 Project Overview

Webserv is a custom HTTP/1.1 server implemented in C++98, capable of serving static websites, handling file uploads, and executing CGI scripts. This project delves into the core mechanics of web servers, providing a deep understanding of the HTTP protocol and network programming.

Unlike higher-level frameworks that abstract away the complexities of HTTP communication, Webserv is built from the ground up, managing everything from socket connections to request parsing and response generation manually.

The server features non-blocking I/O operations, efficient connection handling through poll/select/kqueue/epoll, and support for multiple virtual servers through a Nginx-inspired configuration system.

## ✨ Features

### Core HTTP Server
- **HTTP/1.1 Protocol**: Full implementation of essential HTTP features
- **Multiple Virtual Servers**: Host multiple websites on different ports
- **Non-blocking I/O**: Efficient handling of concurrent connections
- **Connection Management**: Proper timeout handling and connection limits
- **Error Handling**: Custom error pages and proper error status codes

### Request Processing
- **Method Support**: Complete implementation of GET, POST, and DELETE methods
- **Header Parsing**: Accurate parsing and validation of HTTP headers
- **Request Validation**: Proper handling of malformed requests
- **Content Negotiation**: Support for different content types

### Response Generation
- **Status Codes**: Accurate HTTP response status codes
- **MIME Types**: Proper content type detection based on file extensions
- **Response Headers**: Generation of appropriate response headers
- **Chunked Transfer**: Support for chunked transfer encoding

### File Operations
- **Static File Serving**: Efficiently serve HTML, CSS, JS, and media files
- **Directory Listing**: Optional directory browsing for specified routes
- **File Uploads**: Handle multipart/form-data for file uploads
- **Access Control**: Path validation and security measures

### CGI Support
- **CGI Execution**: Run server-side scripts (PHP, Python, etc.)
- **Environment Variables**: Proper setup of CGI environment
- **Input/Output Handling**: Manage data flow between client and CGI script
- **Process Management**: Proper creation and cleanup of CGI processes

## 🔧 Technical Implementation

### Server Architecture
The server follows a modular, event-driven architecture:

1. **Socket Management**: Creating, binding, and listening on specified ports
2. **Event Loop**: Using poll/select/kqueue/epoll for non-blocking I/O operations
3. **Request Parsing**: Breaking down raw HTTP requests into manageable components
4. **Routing**: Matching requests to the appropriate handlers based on configuration
5. **Response Generation**: Creating and sending HTTP responses
6. **Resource Cleanup**: Properly closing connections and freeing resources

### Configuration System
The configuration system is inspired by Nginx and supports:

- Multiple server blocks with different hosts and ports
- Location blocks for route-specific settings
- Custom error pages
- Client body size limits
- Directory listing toggles
- Index file specifications
- CGI execution rules

### Non-blocking I/O
All I/O operations are non-blocking, allowing the server to handle multiple connections simultaneously without spawning multiple threads or processes:

- Socket operations (accept, send, recv) are all non-blocking
- A single poll/select/kqueue/epoll call manages all file descriptors
- Proper buffering mechanisms for partial reads and writes
- Timeout handling for stalled connections

### CGI Implementation
The CGI implementation follows the Common Gateway Interface specification:

- Proper environment variable setup (PATH_INFO, QUERY_STRING, etc.)
- Request body forwarding to CGI scripts
- Response parsing and forwarding to clients
- Process management with appropriate timeout handling

## 💻 Usage

### Prerequisites
- C++ compiler with C++98 support
- Unix-based operating system (Linux or macOS)
- Make

### Installation
```bash
# Clone the repository
git clone https://github.com/Melis-Pablo/webserv.git
cd webserv

# Compile the server
make
```

### Running the Server
```bash
# Start with default configuration
./webserv

# Start with a specific configuration file
./webserv config/default.conf
```

### Configuration Example
```
# Server block example
server {
    listen 8080;
    server_name example.com;
    client_max_body_size 10M;
    error_page 404 /404.html;

    location / {
        root /var/www/html;
        index index.html;
        allowed_methods GET POST;
        autoindex on;
    }

    location /uploads {
        root /var/www/uploads;
        allowed_methods POST;
        upload_store /tmp/uploads;
    }

    location ~ \.php$ {
        root /var/www/php;
        index index.php;
        allowed_methods GET POST;
        cgi_pass /usr/bin/php-cgi;
    }
}
```

## 📝 Learning Outcomes

This project provided in-depth experience with:

- **Network Programming**: Socket management, connection handling, and HTTP protocol implementation
- **Event-driven Architecture**: Non-blocking I/O and event loop design
- **HTTP Protocol**: Detailed understanding of HTTP/1.1 specifications
- **Parser Design**: Creating a robust HTTP request parser
- **Process Management**: Handling CGI script execution
- **Configuration Systems**: Designing and implementing a flexible configuration format
- **Error Handling**: Graceful handling of edge cases and error conditions
- **Resource Management**: Efficient allocation and deallocation of resources

## 🛠️ Testing

The server was tested extensively using:

- Manual testing with browsers (Chrome, Firefox, Safari)
- Curl and wget command-line tools
- Custom test scripts for edge cases
- Siege and ApacheBench for load testing
- Valgrind for memory leak detection

## 🔍 Challenges and Solutions

### Challenge: Concurrent Connection Handling
**Solution**: Implemented non-blocking I/O with poll/select/kqueue/epoll to manage multiple connections with a single thread.

### Challenge: CGI Script Execution
**Solution**: Used fork, pipe, and dup2 to create proper communication channels between the server and CGI scripts.

### Challenge: Parsing Multipart Form Data
**Solution**: Developed a state machine parser that efficiently processes multipart boundaries and extracts file content.

### Challenge: Managing Partial Reads/Writes
**Solution**: Implemented buffer management to handle incomplete reads and writes, ensuring data integrity.

## ⚠️ Note

For detailed project requirements, see the [webserv.md](webserv.md) file.

---

*This project is part of the 42 School Common Core curriculum.*