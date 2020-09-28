FROM ubuntu:20.04

LABEL maintainer="support@resilio.com" \
      vendor="Resilio, Inc." \
      version="2.10.1.1087"

ARG USER=resilio
ARG GROUP=resilio
ARG USERID=555
ARG GROUPID=555
ARG HOMEDIR=/home/${USER}
ARG DISTPATH=${HOMEDIR}/resilio-connect-server
ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.10/server/2.10.1/resilio-connect-console-linux-x64.tar.gz

RUN apt-get update && \
    apt-get install -y libnuma1 libnuma-dev && \
    groupadd -g ${GROUPID} ${GROUP} && \
    useradd -m -u ${USERID} -g ${GROUPID} ${USER}

WORKDIR ${HOMEDIR}

ADD ${URL} mc.tar.gz
RUN tar xf mc.tar.gz && \
    rm mc.tar.gz && \
    chown -R ${USER}:${USER} /home/${USER}

ENV DISTPATH ${DISTPATH}
ENV PATH ${DISTPATH}/nodejs/bin:${PATH}
ENV NODE_ENV production

USER ${USER}
WORKDIR ${DISTPATH}/server

ENTRYPOINT ["node"]
CMD ["--max_old_space_size=3072", "./server.js", "run"]
