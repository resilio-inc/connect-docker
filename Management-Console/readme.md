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

Instead of `-d` flag (deattached mode) you can use `-it` to run container in interactive tty mode.

Important: to stop container use:
```
docker stop -t <grace_period_in_sec> <container_id>
```            
where `-t <grace_period_in_sec>` - seconds to wait for stop before killing it (sending SIGKILL). As result process inside container will receive SIGTERM, and after a grace period (10 sec by default) SIGKILL.

#### Via docker-compose:

```
sudo apt install docker.io docker-compose
sudo usermod -aG docker <your_user_name>
# logout and login

mkdir mc_data
sudo chown -R 1000:1000 mc_data
```

Create `docker-compose.yml` file with the following content
```
version: '3'
services:
  resilio_connect:
    stop_grace_period: 10m
    build: .
    volumes:
      - ./mc_data:/home/resilio/resilio-connect-server/var
      - /etc/localtime:/etc/localtime:ro  # sync container time with host time
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
    restart: always  # optional
```

# run docker-compose in deattached mode
```
docker-compose up -d
```

# or run in interactive mode
```
docker-compose up
```

# to stop container(s)
```
docker-compose stop
```

# to stop and remove container and related resources
```
docker-compose down
```

By using `stop_grace_period` you can specify how long to wait when attempting to stop a container if it doesn’t handle SIGTERM (or whatever stop signal has been specified with stop_signal), before sending SIGKILL. Specified as a duration, for example:
```
stop_grace_period: 1s
stop_grace_period: 1m30s
```
By default, stop waits 10 seconds for the container to exit before sending SIGKILL.

Also you can specify stop_signal statement. It sets an alternative signal to stop the container. By default stop uses SIGTERM. Setting an alternative signal using stop_signal causes stop to send that signal instead, for example:
```
stop_signal: SIGUSR1
```
