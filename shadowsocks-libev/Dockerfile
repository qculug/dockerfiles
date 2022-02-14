FROM golang:alpine AS builder

WORKDIR /build

RUN apk add --no-cache --virtual .build-deps git && \
    git clone https://github.com/shadowsocks/v2ray-plugin.git && \
    cd v2ray-plugin && \
    git checkout $(git describe --abbrev=0 --tags) && \
    go build

FROM alpine:latest

LABEL maintainer="Roald Clark <roaldclark@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/qculug/dockerfiles"

RUN echo 'https://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories
RUN apk add --no-cache shadowsocks-libev

ARG HOME="/home/shadowsocks-libev"

RUN adduser -S shadowsocks-libev -G nobody

COPY --from=builder /build/v2ray-plugin/v2ray-plugin /usr/local/bin/v2ray-plugin

RUN chmod +x /usr/local/bin/v2ray-plugin
RUN chown -R shadowsocks-libev:nobody $HOME

USER shadowsocks-libev

WORKDIR $HOME

RUN echo "{}" > $HOME/config.json

CMD ["sh", "-c", "/usr/bin/ss-server -c $HOME/config.json"]
