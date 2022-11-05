#
# ckatsak, Mon 01 Aug 2022 08:56:49 PM EEST
#
# Notes:
# - https://www.gnu.org/software/make/manual/html_node/Target_002dspecific.html

DOCKER ?= docker

RUST_VERSION   := 1.65.0
PROTOC_VERSION := 21.9
IMG_NAME := ckatsak/protoc-rust

IMG_TAG = $(RUST_VERSION)-$(PROTOC_VERSION)-$(OS_VERSION)

TARGETS := bullseye alpine


.PHONY: all build push $(TARGETS)
all:
	@for t in $(TARGETS); do $(MAKE) $$t; done

bullseye: override OS_VERSION = bullseye
alpine:   override OS_VERSION = alpine
$(TARGETS): build push


build:
	$(DOCKER) build \
		--no-cache \
		--progress=plain \
		--pull \
		--file $(CURDIR)/$(OS_VERSION)/Dockerfile \
		--build-arg RUST_VERSION=$(RUST_VERSION) \
		--build-arg PROTOC_VERSION=$(PROTOC_VERSION) \
		--tag $(IMG_NAME):$(IMG_TAG) .
	$(DOCKER) image prune --force --filter label=stage=builder

push:
	$(DOCKER) push $(IMG_NAME):$(IMG_TAG)

