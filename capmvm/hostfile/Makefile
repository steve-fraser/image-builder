DOCKER := docker
BUILD_DATE:=$(shell date --rfc-3339=seconds)
GIT_SHA:=$(shell git rev-parse HEAD)

REGISTRY?=ghcr.io/steve-fraser/
IMAGE_NAME?=$(REGISTRY)/firecracker-hostfile:
TAG?=$(shell git rev-parse --short HEAD)

RELEASE_VERSIONS?=latest

build: $(addprefix build-,$(RELEASE_VERSIONS))
build-%:
	$(DOCKER) build -t $(IMAGE_NAME):$*  \
		.


push: $(addprefix push-,$(RELEASE_VERSIONS))
push-%:
	$(DOCKER) push $(IMAGE_NAME):$*
