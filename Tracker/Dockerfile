FROM ubuntu:18.04

ARG URL=https://download-cdn.resilio.com/resilio-connect-builds-2.6/tracker/2.6/tracker.tar.gz

RUN useradd -Um resilio
WORKDIR /home/resilio

ADD ${URL} tracker.tar.gz
RUN tar xf tracker.tar.gz && \
    rm tracker.tar.gz && \
    mv resilio_tracker/tracker . && \
    rm -rf resilio_tracker
RUN chown -R resilio:resilio /home/resilio

USER resilio
ENTRYPOINT ["./tracker"]
CMD ["-p", "3000"]