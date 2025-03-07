ARG IMAGE_NAME
FROM ${IMAGE_NAME}:dev AS builder

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

RUN make INSTALL_HDR_PATH=/lib/modules/5.15.0-100-generic LOCALVERSION= olddefconfig
RUN make INSTALL_HDR_PATH=/lib/modules/5.15.0-100-generic LOCALVERSION= -j `nproc` all
RUN make INSTALL_HDR_PATH=/lib/modules/5.15.0-100-generic LOCALVERSION= modules_install

RUN cp arch/x86/boot/compressed/vmlinux.bin /boot/vmlinux.bin && \
    cp .config /boot/config-${KERNEL_VERSION}

FROM scratch
COPY --from=builder /boot /boot
COPY --from=builder /lib/modules /lib/modules
