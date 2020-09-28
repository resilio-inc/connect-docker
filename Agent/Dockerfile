FROM ubuntu:20.04

LABEL maintainer="support@resilio.com" \
      vendor="Resilio, Inc." \
      version="2.10.1.1684"

ARG USER=resilio
ARG GROUP=resilio
ARG USERID=555
ARG GROUPID=555
ARG HOMEDIR=/home/${USER}
ARG STORAGEDIR=${HOMEDIR}/.sync
ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.10/agent/2.10.1.1684/resilio-connect-agent_x64.tar.gz

RUN apt-get update && \
    apt-get install -y libnuma1 libnuma-dev && \
    groupadd -g ${GROUPID} ${GROUP} && \
    useradd -m -u ${USERID} -g ${GROUPID} ${USER}

WORKDIR ${HOMEDIR}

ADD ${URL} agent.tar.gz
RUN tar xf agent.tar.gz && \
    rm agent.tar.gz && \
    if [ -f "./get-rslpm" ]; then chmod +x ./get-rslpm && mv ./get-rslpm /bin; fi && \
    mkdir ${STORAGEDIR} && \
    chown -R ${USER}:${GROUP} ${HOMEDIR}

USER ${USER}
ENTRYPOINT ["./rslagent"]
CMD ["--config", "/home/resilio/sync.conf", "--nodaemon", "--log", "/home/resilio/.sync/sync.log"]
