# Resilio Connect Management Console in Docker

### Preparation

- Install Docker and add your current user to docker group:
```
sudo apt install docker.io
sudo usermod -aG docker ${USER}
```
You need to logout and then login back for the changes to take effect. 
Or you can work with Docker using `sudo` command.

### Steps to build Management Console Docker image
```
docker build -t resilio_mc .
```

##### Important:

Management console is running under `resilio` user inside container which has user ID `555` and group ID `555` (see Dockerfile). 
So need to make sure the storage folder (`mc_data`) has owner and group `555` on the host system (described below).

#### Steps to run Management Console Docker container
```
mkdir mc_data
sudo chown 555:555 ./mc_data

docker run -d -v $(pwd)/mc_data:/home/resilio/resilio-connect-server/var \
           -v /etc/localtime:/etc/localtime:ro \
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
           resilio_mc
```

##### Important: Docker on Linux allows you to run container in (["host" network mode](https://docs.docker.com/network/network-tutorial-host/)), so there will no separate isolated network stack the container work with, it will use network stack of the host PC. Thus you don't need specify `-p` parameters and additionally adjust NAT on host PC. 
```
docker run -d --network host \
              -v $(pwd)/mc_data:/home/resilio/resilio-connect-server/var \
              -v /etc/localtime:/etc/localtime:ro \
              --restart=always \
              resilio_mc
```

Instead of `-d` flag you can use `-it` to run container in interactive TTY mode.

To stop container use:
```
docker stop -t <grace_period_in_sec> <container_id>
```

By `docker stop` the main process inside the container will receive `SIGTERM`, and after a grace period, `SIGKILL`. 
By default, stop waits 10 seconds for the container to exit before sending `SIGKILL`. 
`<grace_period_in_sec>` - seconds to wait for stop before killing it (sending `SIGKILL`).
