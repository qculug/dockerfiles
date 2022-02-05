FROM golang:alpine AS builder

WORKDIR /build

RUN apk add --no-cache --virtual .build-deps git make && \
    git clone --branch develop https://github.com/qculug/proxypool.git && \
    cd proxypool && \
    go mod download && \
    make docker

FROM alpine:latest

LABEL maintainer="Roald Clark <roaldclark@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/qculug/dockerfiles

RUN apk add --no-cache ca-certificates tzdata

ARG HOME="/home/proxypool"

RUN adduser -S proxypool -G nobody

COPY --from=builder /build/proxypool/bin/proxypool-docker "${HOME}"/proxypool
COPY --from=builder /build/proxypool/assets "${HOME}"/assets
COPY --from=builder /build/proxypool/config/config.yaml "${HOME}"/config.yaml
COPY --from=builder /build/proxypool/config/source.yaml "${HOME}"/config/source.yaml

RUN chown -R proxypool:nobody "${HOME}"

USER proxypool

WORKDIR "${HOME}"

CMD ["sh", "-c", "${HOME}/proxypool -d"]
