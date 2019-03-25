FROM ubuntu:18.04

ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.6/server/2.6.2/resilio-connect-console-linux-x64.tar.gz

RUN useradd -Um resilio
WORKDIR /home/resilio

ADD ${URL} mc.tar.gz
RUN tar xf mc.tar.gz && rm mc.tar.gz
RUN chown -R resilio:resilio /home/resilio

USER resilio
WORKDIR /home/resilio/resilio-connect-server
ENTRYPOINT exec ./srvctrl run