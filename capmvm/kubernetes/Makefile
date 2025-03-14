DOCKER := docker
BUILD_DATE:=$(shell date --rfc-3339=seconds)
GIT_SHA:=$(shell git rev-parse HEAD)

REGISTRY?=ghcr.io/steve-fraser
IMAGE_NAME?=$(REGISTRY)/capmvm-k8s-os-ch
CONTAINERD_VERSION?=1.6.34
TAG?=$(shell git rev-parse --short HEAD)

RELEASE_VERSIONS?=1.31.6

build: $(addprefix build-,$(RELEASE_VERSIONS))
build-%:
	$(DOCKER) build -t $(IMAGE_NAME):$*  \
	--build-arg KUBERNETES_VERSION=$* \
	--build-arg CONTAINERD_VERSION=$(CONTAINERD_VERSION) \
	--label "org.opencontainers.image.created"="$(BUILD_DATE)" \
	--label "org.opencontainers.image.authors"="Liquid Metal Authors" \
	--label "org.opencontainers.image.url"="https://github.com/weaveworks-liquidmetal/image-builder" \
	--label "org.opencontainers.image.source"="https://github.com/weaveworks-liquidmetal/image-builder/tree/main/capmvm/kubernetes" \
	--label "org.opencontainers.image.revision"="$(GIT_SHA)" \
	--label "org.opencontainers.image.vendor"="Weaveworks" \
	--label "org.opencontainers.image.licenses"="MPL-2.0" \
	--label "org.opencontainers.image.title"="Cluster API Provider Microvm Kubernetes v$*" \
	--label "org.opencontainers.image.description"="A image to use for a microvm volume when creating a cluster using Cluster API Provider Microvm" \
	--label "works.weave.liquidmetal.kubernetes"="$*" \
	--label "works.weave.liquidmetal.containerd"="$(CONTAINERD_VERSION)" \
	.
	$(DOCKER) build -t $(IMAGE_NAME)-nv-gpu:$*  \
		--file Dockerfile.nv-gpu \
		--build-arg IMAGE=$(IMAGE_NAME):$*  \
		--label "org.opencontainers.image.created"="$(BUILD_DATE)" \
		--label "org.opencontainers.image.authors"="Liquid Metal Authors" \
		--label "org.opencontainers.image.url"="https://github.com/weaveworks-liquidmetal/image-builder" \
		--label "org.opencontainers.image.source"="https://github.com/weaveworks-liquidmetal/image-builder/tree/main/capmvm/kubernetes" \
		--label "org.opencontainers.image.revision"="$(GIT_SHA)" \
		--label "org.opencontainers.image.vendor"="Weaveworks" \
		--label "org.opencontainers.image.licenses"="MPL-2.0" \
		--label "org.opencontainers.image.title"="Cluster API Provider Microvm Kubernetes v$*" \
		--label "org.opencontainers.image.description"="A image to use for a microvm volume when creating a cluster using Cluster API Provider Microvm" \
		--label "works.weave.liquidmetal.kubernetes"="$*" \
		--label "works.weave.liquidmetal.containerd"="$(CONTAINERD_VERSION)" \
		.
push: $(addprefix push-,$(RELEASE_VERSIONS))
push-%:
	$(DOCKER) push $(IMAGE_NAME):$*
	$(DOCKER) push $(IMAGE_NAME)-nv-gpu:$*
