## Resilio Connect agent

### Preparation

- Install Docker and add your current user to docker group:
```
sudo apt install docker.io
sudo usermod -aG docker <username>
```
then logout and login back.

### Steps to build and run container on Linux

```
docker build -t resilio_connect_agent .

mkdir shared storage
sudo chown -R 1000:1000 shared storage

docker run -d -p 3839:3839/tcp \  
              -p 3838:3838/tcp \  
              -p 3839:3839/udp \  
              -p 3838:3838/udp \  
              -v $(pwd)/sync.conf:/home/resilio/sync.conf \
              -v $(pwd)/shared:/home/resilio/shared \
              -v $(pwd)/storage:/home/resilio/.sync \
              --restart always \
              resilio_connect_agent
```