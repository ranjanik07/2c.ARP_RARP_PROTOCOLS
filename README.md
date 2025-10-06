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
## SERVER
```import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening...")

c, addr = s.accept()
print(f"Connection established with {addr}")

address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
}

while True:
    ip = c.recv(1024).decode()

    if not ip:  
        break

    try:
        mac = address[ip]  # Get the MAC address for the IP
        print(f"IP: {ip} -> MAC: {mac}")
        c.send(mac.encode())  
    except KeyError:
        print(f"IP: {ip} not found in ARP table.")
        c.send("Not Found".encode())
c.close()
s.close()
```
## CLIENT
```
import socket
c = socket.socket()
c.connect(('localhost', 8000))

while True:
    ip = input("Enter IP address to find MAC (or type 'exit' to quit): ")

    if ip.lower() == "exit":  
        break

    c.send(ip.encode())
    mac = c.recv(1024).decode()
    print(f"MAC Address for {ip}: {mac}")
c.close()
```
## OUPUT - ARP
<img width="595" height="120" alt="Screenshot 2025-10-06 104646" src="https://github.com/user-attachments/assets/765702ac-afce-4fb3-8fc7-5f17d412c7f3" />
<img width="856" height="114" alt="Screenshot 2025-10-06 104639" src="https://github.com/user-attachments/assets/0383ccec-e782-4921-9ed0-e23ab7a15b6a" />

## PROGRAM - RARP
## SERVER
```
import socket

s = socket.socket()
s.connect(('localhost', 9000))

while True:
    mac = input("Enter MAC Address: ")
    if not mac:
        break
    s.send(mac.encode())
    print("Logical Address:", s.recv(1024).decode())

s.close()
```
## CLIENT
```
import socket

s = socket.socket()
s.bind(('localhost', 9000))
s.listen(5)

print("RARP Server is running... Waiting for request...")
c, addr = s.accept()
print("Connection established with:", addr)

address = {
    "B8:1E:A4:E2:22:E3": "192.168.1.100"
}

while True:
    mac = c.recv(1024).decode()
    if not mac:
        break
    try:
        c.send(address[mac].encode())
    except KeyError:
        c.send("Not Found".encode())

c.close()
```
## OUPUT -RARP
<img width="622" height="61" alt="Screenshot 2025-10-06 104134" src="https://github.com/user-attachments/assets/0c6fde4d-134f-4a38-af76-4fb436bc62d4" />
<img width="453" height="98" alt="Screenshot 2025-10-06 104110" src="https://github.com/user-attachments/assets/12267cad-8e82-4d91-96d7-7e4cd3331bc2" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
