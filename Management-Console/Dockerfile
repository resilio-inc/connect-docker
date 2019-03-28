FROM ubuntu:18.04

ARG USER=resilio

RUN useradd -Um $USER
WORKDIR /home/$USER

ADD https://download-cdn.resilio.com/resilio-connect-builds-2.6/server/2.6.2/resilio-connect-console-linux-x64.tar.gz mc.tar.gz
RUN tar xf mc.tar.gz && rm mc.tar.gz
RUN chown -R $USER:$USER /home/$USER

ARG DIST_PATH=/home/$USER/resilio-connect-server

USER $USER
WORKDIR $DIST_PATH

ENV DIST_PATH $DIST_PATH
ENV SERVER_DIST_PATH $DIST_PATH/server
ENV PATH $DIST_PATH/nodejs/bin:$PATH
ENV NODE_ENV production

ENTRYPOINT exec node --max_old_space_size=3072 "$SERVER_DIST_PATH/server.js" run
