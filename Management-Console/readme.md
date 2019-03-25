# Resilio Connect Management Console

### Steps to run Management Console in Docker

##### Important:

Management console is running under `resilio` user inside container and it has `UID=1000` and `GID=1000`.

#### Via pure Docker:

```
sudo apt install docker.io
sudo usermod -aG docker <your_user_name>
# logout and login

docker build -t resilio_connect_mc .

mkdir mc_data
sudo chown -R 1000:1000 mc_data

# run docker in deattached mode
docker run -d -v $(pwd)/mc_data:/home/resilio/resilio-connect-server/var \
           -p 8443:8443/tcp \
           -p 8444:8444/tcp \
           -p 8445:8445/tcp \
           -p 3000:3000/tcp \
           -p 3000:3000/udp \
           -p 3839:3839/tcp \
           -p 3839:3839/udp \
           -p 3838:3838/tcp \
           -p 3838:3838/udp \
           --restart=always \
           resilio_connect_mc
```

Instead of `-d` flag you can use `-it` to run container in interactive tty mode.

#### Via docker-compose:

```
sudo apt install docker.io docker-compose
sudo usermod -aG docker <your_user_name>
# logout and login

docker build -t resilio_connect_mc .

mkdir mc_data
sudo chown -R 1000:1000 mc_data
```

Create docker-compose.yml file with the following content

```
version: '2'
services:
  resilio_connect:
    image: resilio_connect_mc
    volumes:
      - ./mc_data:/home/resilio/resilio-connect-server/var
    ports:
      - 8445:8445/tcp
      - 8444:8444/tcp
      - 8443:8443/tcp
      - 3000:3000/tcp
      - 3000:3000/udp
      - 3838:3838/tcp
      - 3838:3838/udp
      - 3839:3839/tcp
      - 3839:3839/udp
    restart: always
```

Run docker-compose in deattached mode
```
docker-compose up -d
```