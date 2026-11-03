# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
Client
~~~
import socket

s = socket.socket()
s.bind(('127.0.0.1', 55000))
s.listen(1)

print("Waiting for server...")
c, addr = s.accept()

size = int(input("Enter number of frames to send: "))
w = int(input("Enter Window Size: "))

frames = list(range(1, size + 1))

for i in range(0, size, w):
    window = frames[i:i+w]

    print("Sending frames:", window)
    c.sendall((str(window) + "\n").encode())

    ack = c.recv(1024).decode()
    print("Server:", ack)

c.sendall("END\n".encode())

c.close()
s.close()
~~~
Server
~~~
import socket

s = socket.socket()
s.connect(('127.0.0.1', 55000))

f = s.makefile('r')

while True:
    data = f.readline().strip()

    if not data or data == "END":
        break

    print("Received frames:", data)

    s.sendall("ACK Received".encode())

s.close()
~~~

## OUPUT
<img width="562" height="305" alt="image" src="https://github.com/user-attachments/assets/cb993709-afd5-4100-aab8-adab85e8b7ea" />

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
