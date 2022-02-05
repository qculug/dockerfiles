FROM golang:alpine AS builder

WORKDIR /build

RUN apk add --no-cache --virtual .build-deps git && \
    git clone https://gitlab.com/yawning/obfs4.git && \
    cd obfs4 && \
    go build -o obfs4proxy/obfs4proxy ./obfs4proxy

FROM alpine:latest

LABEL maintainer="Roald Clark <roaldclark@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/qculug/dockerfiles"

RUN echo 'https://dl-cdn.alpinelinux.org/alpine/edge/community' >> /etc/apk/repositories
RUN apk add --no-cache tor && \
    chmod 700 /var/lib/tor

COPY --from=builder /build/obfs4/obfs4proxy /usr/local/bin/obfs4proxy

RUN chmod +x /usr/local/bin/obfs4proxy

USER tor

CMD ["/usr/bin/tor", "-f", "/etc/tor/torrc"]
