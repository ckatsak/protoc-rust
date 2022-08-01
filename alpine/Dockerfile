# syntax=docker/dockerfile:1

ARG PROTOC_VERSION
ARG RUST_VERSION

FROM rust:$RUST_VERSION-alpine as builder
LABEL stage=builder
ARG PROTOC_VERSION
# repeated ARG to evaluate it inside the subsequent RUN too (https://stackoverflow.com/a/56748289)

# Install protoc, based on:
# - https://github.com/protocolbuffers/protobuf/blob/c9869dc7803eb0a21d7e589c40ff4f9288cd34ae/src/README.md
ADD https://github.com/protocolbuffers/protobuf/releases/download/v$PROTOC_VERSION/protobuf-python-4.$PROTOC_VERSION.tar.gz /tmp

RUN apk update \
    && apk add --no-cache --update autoconf automake libtool make g++ unzip file musl-dev \
    && tar -C /tmp -xf /tmp/protobuf-python-4.$PROTOC_VERSION.tar.gz \
    && cd /tmp/protobuf-4.$PROTOC_VERSION \
    && ./configure --prefix=/tmp/protoc \
    && make -j$(nproc) \
    && make -j$(nproc) check \
    && make install \
    && rustup component add rustfmt


FROM rust:$RUST_VERSION-alpine
ARG PROTOC_VERSION
LABEL maintainer="ckatsak@gmail.com" \
    version=$RUST_VERSION-$PROTOC_VERSION-alpine

COPY --from=builder /tmp/protoc/bin/protoc /usr/local/bin/
COPY --from=builder /tmp/protoc/include/ /usr/local/include/
COPY --from=builder /tmp/protoc/lib/pkgconfig/ /usr/local/lib/pkgconfig/
COPY --from=builder /tmp/protoc/lib/ /usr/local/lib/
