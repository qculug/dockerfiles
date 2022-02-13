FROM golang:alpine AS builder

WORKDIR /build

RUN apk add --no-cache --virtual .build-deps git && \
    git clone https://github.com/v2fly/v2ray-core.git && \
    cd v2ray-core && \
    git checkout $(git describe --abbrev=0 --tags) && \
    go mod download && \
    go build -o v2ray ./main

FROM alpine:latest

LABEL maintainer="Roald Clark <roaldclark@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/qculug/dockerfiles"

ARG HOME="/home/v2ray"

RUN adduser -S v2ray -G nobody

COPY --from=builder /build/v2ray-core/v2ray /usr/local/bin/v2ray
COPY --from=builder /build/v2ray-core/release/config/config.json $HOME/config.json

RUN chmod +x /usr/local/bin/v2ray
RUN chown -R v2ray:nobody $HOME

USER v2ray

WORKDIR $HOME

CMD ["sh", "-c", "/usr/local/bin/v2ray -config $HOME/config.json"]
