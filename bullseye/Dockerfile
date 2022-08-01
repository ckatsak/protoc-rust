# syntax=docker/dockerfile:1

ARG PROTOC_VERSION
ARG RUST_VERSION

FROM rust:$RUST_VERSION-bullseye as builder
LABEL stage=builder
ARG PROTOC_VERSION
# repeated ARG to evaluate it inside the subsequent RUN too (https://stackoverflow.com/a/56748289)

# Install protoc, based on:
# - https://grpc.io/docs/protoc-installation/#install-pre-compiled-binaries-any-os
ADD https://github.com/protocolbuffers/protobuf/releases/download/v$PROTOC_VERSION/protoc-$PROTOC_VERSION-linux-x86_64.zip /tmp/
RUN unzip /tmp/protoc-$PROTOC_VERSION-linux-x86_64.zip -d /tmp/protoc


FROM rust:$RUST_VERSION-slim-bullseye
ARG PROTOC_VERSION
LABEL maintainer="ckatsak@gmail.com" \
    version=$RUST_VERSION-$PROTOC_VERSION-bullseye

COPY --from=builder /tmp/protoc/bin/protoc /usr/local/bin/
COPY --from=builder /tmp/protoc/include/ /usr/local/include/
