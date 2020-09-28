## Resilio Connect Tracker

### Preparation

- Install Docker and add your current user to docker group:
```
sudo apt install docker.io
sudo usermod -aG docker ${USER}
```
You need to logout and then login back for the changes to take effect. 
Or you can work with Docker using `sudo` command.

### Steps to build Tracker Docker image
```
docker build -t resilio_tracker .
```

### Steps to run container on Linux
```
docker run -d -p 3000:3000/tcp -p 3000:3000/udp --restart always resilio_tracker
```