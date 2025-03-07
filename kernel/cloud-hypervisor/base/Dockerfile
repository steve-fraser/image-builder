ARG IMAGE_NAME
FROM ${IMAGE_NAME}:dev AS builder

RUN apt update && apt install -y rsync

ENV SRC_DIR=/usr/src            \
    DIST_DIR=/dist              \
    LINUX_DIR=/usr/src/linux    \
    LINUX_REPO_URL=https://github.com/torvalds/linux.git

ARG KERNEL_VERSION

RUN mkdir -p ${SRC_DIR} ${DIST_DIR} && \
    git clone --depth 1 --branch v${KERNEL_VERSION} ${LINUX_REPO_URL} ${LINUX_DIR} && \
    cd ${LINUX_DIR}

WORKDIR ${LINUX_DIR}

COPY cloud-hypervisor/configs/linux-x86_64-${KERNEL_VERSION}.config .config

RUN make LOCALVERSION=-97-generic olddefconfig
RUN make LOCALVERSION=-97-generic -j `nproc` all
RUN make LOCALVERSION=-97-generic modules_install
RUN ls /usr/src/

RUN cp arch/x86/boot/compressed/vmlinux.bin /boot/vmlinux.bin && \
    cp .config /boot/config-${KERNEL_VERSION}

FROM scratch
COPY --from=builder /boot /boot
COPY --from=builder /lib/modules /lib/modules