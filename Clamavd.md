This is the process to host the container

Dockerfile
```
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y clamav clamav-daemon && \
    # Add TCP socket and address config (explicit, avoids sed issues)
    echo "TCPSocket 3310" >> /etc/clamav/clamd.conf && \
    echo "TCPAddr 0.0.0.0" >> /etc/clamav/clamd.conf && \
    sed -i 's/^Example/#Example/' /etc/clamav/clamd.conf && \
    # Fix permissions and run freshclam
    mkdir -p /var/run/clamav && \
    chown clamav:clamav /var/run/clamav && \
    freshclam

EXPOSE 3310

CMD ["clamd", "--foreground", "-c", "/etc/clamav/clamd.conf"]
```

```
docker build -t clamav-fuzzer .
docker rm -f clamavd
docker run -d --name clamavd -p 3310:3310 clamav-fuzzer
```

Output:
```
TCPSocket 3310
TCPAddr 0.0.0.0
```

```
tcp        0      0 0.0.0.0:3310          0.0.0.0:*               LISTEN      <pid>/clamd
```


Check connection:
clamd_connectioncheck.py
```
import socket

try:
    s = socket.create_connection(("localhost", 3310), timeout=5)
    s.sendall(b"PING\n")
    response = s.recv(1024)
    print("Response:", response.decode())
    s.close()
except Exception as e:
    print("Connection failed:", e)
```

Output:
```
Response: PONG
```
