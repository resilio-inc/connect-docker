FROM ubuntu:18.04

ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.6/agent/2.6.2/resilio-connect-agent_x64.tar.gz

RUN useradd -Um resilio
WORKDIR /home/resilio

ADD ${URL} agent.tar.gz
RUN tar xf agent.tar.gz && rm agent.tar.gz
RUN chown -R resilio:resilio /home/resilio

USER resilio
RUN mkdir .sync
ENTRYPOINT ["./rslagent"]
CMD ["--config", "/home/resilio/sync.conf", "--nodaemon", "--log", "/home/resilio/.sync/sync.log"]