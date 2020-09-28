## Resilio Connect Agent in Docker

### Preparation

- Install Docker and add your current user to docker group:
```
sudo apt install docker.io
sudo usermod -aG docker ${USER}
```
You need to logout and then login back for the changes to take effect. 
Or you can work with Docker using `sudo` command.

### Steps to build Agent Docker image
```
docker build -t resilio_agent .
```

#### Important!
- `555` is user ID and group ID of `resilio` user agent is running as inside the container (see Dockerfile), 
so all mounted folders must have `555:555` permissions on a host system.

- You need to generate `sync.conf` agent config first in the Management Console. 

### Steps to run container
```
mkdir <full_path_to_shared_dir_on_host>
mkdir <full_path_to_agent_storage_folder>

sudo chown -R 555:555 <full_path_to_shared_dir_on_host>
sudo chown -R 555:555 <full_path_to_agent_storage_folder>

docker run -d -p 3839:3839/tcp \
              -p 3838:3838/tcp \
              -p 3839:3839/udp \
              -p 3838:3838/udp \
              -v <full_full_path_to_sync.conf>:/home/resilio/sync.conf \
              -v <full_path_to_shared_dir_on_host>:/home/resilio/shared \
              -v <full_path_to_agent_storage_folder>:/home/resilio/.sync \
              -v /etc/localtime:/etc/localtime:ro \
              --restart always \
              resilio_agent
```

For example:
```
mkdir shared storage
sudo chown -R 555:555 ./shared ./storage
docker run -d -p 3849:3839/tcp \
              -p 3848:3838/tcp \
              -p 3849:3839/udp \
              -p 3848:3838/udp \
              -v $(pwd)/sync.conf:/home/resilio/sync.conf \
              -v $(pwd)/shared:/home/resilio/shared \
              -v $(pwd)/storage:/home/resilio/.sync \
              -v /etc/localtime:/etc/localtime:ro \
              --restart always \
              resilio_agent
```

##### Important: Docker on Linux allows you to run container in (["host" network mode](https://docs.docker.com/network/network-tutorial-host/)), so there will no separate isolated network stack the container work with, it will use network stack of the host PC. Thus you don't need specify `-p` parameters and additionally adjust NAT on host PC. 
```
docker run -d --network host \
              -v $(pwd)/sync.conf:/home/resilio/sync.conf \
              -v $(pwd)/shared:/home/resilio/shared \
              -v $(pwd)/storage:/home/resilio/.sync \
              -v /etc/localtime:/etc/localtime:ro \
              --restart always \
              resilio_agent
```

It's recommended to use `host` network mode in case of high load, because in this case Linux on agent's PC won't consume CPU and RAM to support additional network stack and additional internal NAT.
