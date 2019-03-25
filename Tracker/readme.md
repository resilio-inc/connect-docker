## Resilio Connect tracker

### Steps to run container on Linux

```
docker build -t resilio_connect_tracker .
docker run -d -p 3000:3000/tcp -p 3000:3000/udp --restart always resilio_connect_tracker
```