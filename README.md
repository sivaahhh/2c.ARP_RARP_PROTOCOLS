# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
```
## SERVER-ARP:
import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("ARP Server is listening on port 8000...")
c, addr = s.accept()

address = {
    "148.129.123.45": "5D:BC:E3:FA",
    "109.178.123.45": "8A:2B:3C:4D",
}

while True:
    ip = c.recv(1024).decode()
    print(f"Received IP: {ip}")
    mac = address.get(ip, "Not Found")
    c.send(mac.encode())
## CLIENT-ARP:
import socket

s = socket.socket()
s.connect(('localhost', 8000))

while True:
    ip = input("Enter Logical Address (IP): ")
    s.send(ip.encode())
    print("MAC Address:", s.recv(1024).decode())

```
## OUPUT - ARP
Server-ARP:
<img width="1048" height="167" alt="Screenshot 2026-05-15 154152" src="https://github.com/user-attachments/assets/44214ce6-fd19-4e89-8610-ae76fed870ad" />
Client-ARP:
<img width="1048" height="203" alt="Screenshot 2026-05-15 154202" src="https://github.com/user-attachments/assets/d464117b-a140-4f38-a418-952df285c529" />

## PROGRAM - RARP
## SERVER-RARP:
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening for RARP requests...")
c, addr = s.accept()
print(f"Connection established with {addr}")

rarp_table = {
    "6A:08:AA:C2": "165.165.80.80",
    "8A:BC:E3:FA": "165.165.79.1"
}

while True:
    mac = c.recv(1024).decode()

    if not mac:  
        break

    try:
        ip = rarp_table[mac]  
        print(f"MAC: {mac} -> IP: {ip}")
        c.send(ip.encode())  
    except KeyError:
        print(f"MAC: {mac} not found in RARP table.")
        c.send("Not Found".encode())
c.close()
s.close()
## CLIENT RARP:
import socket
c = socket.socket()
c.connect(('localhost', 8000))

while True:
    mac = input("Enter MAC address to find IP (or type 'exit' to quit): ")
    if mac.lower() == "exit":  
        break
    c.send(mac.encode())
    ip = c.recv(1024).decode()
    print(f"IP Address for {mac}: {ip}")
c.close()
## OUPUT -RARP
Server-RARP
<img width="1046" height="185" alt="Screenshot 2026-05-15 154629" src="https://github.com/user-attachments/assets/bb943ae8-2481-483a-bdee-963b5b5f1d3a" />

Client-RARP
<img width="1002" height="163" alt="Screenshot 2026-05-15 154648" src="https://github.com/user-attachments/assets/8b22bd8d-ce38-43d7-8eb4-10dc58818337" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
