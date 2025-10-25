## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
server:
```
import socket
import os

# Server Configuration
HOST = '127.0.0.1'  # Localhost
PORT = 8080

# Create TCP Socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))
server_socket.listen(5)
print(f"HTTP Server started on {HOST}:{PORT}")

while True:
    client_socket, client_addr = server_socket.accept()
    print(f"Connection from {client_addr}")

    request = client_socket.recv(1024).decode()
    print(f"Received request:\n{request}")

    if request.startswith("GET"):
        # Extract requested filename
        filename = request.split()[1].lstrip('/')
        if os.path.exists(filename):
            with open(filename, 'r') as f:
                content = f.read()
            response = "HTTP/1.1 200 OK\n\n" + content
        else:
            response = "HTTP/1.1 404 Not Found\n\nFile Not Found"
        client_socket.send(response.encode())
    
    elif request.startswith("POST"):
        # Simulate receiving a file
        try:
            header, content = request.split('\n\n', 1)
            filename = header.split()[1].lstrip('/')
            with open(filename, 'w') as f:
                f.write(content)
            ack = "HTTP/1.1 200 OK\n\nACK: File uploaded successfully"
        except:
            ack = "HTTP/1.1 400 Bad Request\n\nNACK: Failed to upload file"
        client_socket.send(ack.encode())
    
    client_socket.close()
```
client:
```
import socket

HOST = '127.0.0.1'  # Server IP
PORT = 8080

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((HOST, PORT))

choice = input("Enter 1 to download page (GET), 2 to upload page (POST): ")

if choice == '1':
    filename = input("Enter filename to download: ")
    request = f"GET /{filename} HTTP/1.1\nHost: {HOST}\n\n"
    client_socket.send(request.encode())
    response = client_socket.recv(4096).decode()
    print("Server Response:\n", response)

elif choice == '2':
    filename = input("Enter filename to upload: ")
    with open(filename, 'r') as f:
        content = f.read()
    request = f"POST /{filename} HTTP/1.1\nHost: {HOST}\n\n{content}"
    client_socket.send(request.encode())
    response = client_socket.recv(1024).decode()
    print("Server Response:\n", response)

client_socket.close()
```

## OUTPUT
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/524036c2-195f-4d28-aa0d-ce1bf43658cd" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
