FROM alpine:latest

LABEL maintainer="Roald Clark <roaldclark@gmail.com>"

RUN echo 'https://dl-cdn.alpinelinux.org/alpine/edge/community' >> /etc/apk/repositories
RUN echo 'https://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories
RUN apk add --no-cache tor obfs4proxy && \
    chmod 700 /var/lib/tor

USER tor

CMD ["/usr/bin/tor", "-f", "/etc/tor/torrc"]
