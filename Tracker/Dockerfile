FROM ubuntu:20.04

LABEL maintainer="support@resilio.com" \
      vendor="Resilio, Inc." \
      version="3.0.454"

ARG USER=resilio
ARG GROUP=resilio
ARG USERID=555
ARG GROUPID=555
ARG HOMEDIR=/home/${USER}
ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.10/tracker/3.0/tracker.tar.gz

RUN groupadd -g ${GROUPID} ${GROUP} && \
    useradd -m -u ${USERID} -g ${GROUPID} ${USER}

WORKDIR ${HOMEDIR}

ADD ${URL} tracker.tar.gz
RUN tar xf tracker.tar.gz && \
    rm tracker.tar.gz && \
    mv resilio_tracker/tracker . && \
    rm -rf resilio_tracker && \
    chown -R ${USERID}:${GROUPID} ${HOMEDIR}

USER ${USER}
ENTRYPOINT ["./tracker"]
CMD ["-p", "3000"]
