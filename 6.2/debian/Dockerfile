FROM registry.drycc.cc/drycc/base:bullseye

ENV GOSU_VERSION="1.14" \
    REDIS_SENTINEL_VERSION="6.2.6"


COPY prebuildfs /

RUN install-packages libnss-wrapper \
    && install-stack gosu $GOSU_VERSION \
    && install-stack redis-sentinel $REDIS_SENTINEL_VERSION \
    && echo "export NSS_WRAPPER_LIB=/usr/lib/`echo $(uname -m)`-linux-gnu/libnss_wrapper.so" >> /opt/drycc/redis-sentinel/profile.d/redis-sentinel.sh \
    && . init-stack \
    && rm -rf \
    /usr/share/doc \
    /usr/share/man \
    /usr/share/info \
    /usr/share/locale \
    /var/lib/apt/lists/* \
    /var/log/* \
    /var/cache/debconf/* \
    /etc/systemd \
    /lib/lsb \
    /lib/udev \
    /usr/lib/`echo $(uname -m)`-linux-gnu/gconv/IBM* \
    /usr/lib/`echo $(uname -m)`-linux-gnu/gconv/EBC* \
    && mkdir -p /usr/share/man/man{1..8}

COPY rootfs /
RUN /opt/drycc/scripts/redis-sentinel/postunpack.sh

EXPOSE 26379

USER 1001
ENTRYPOINT [ "init-stack", "/opt/drycc/scripts/redis-sentinel/entrypoint.sh" ]
CMD [ "/opt/drycc/scripts/redis-sentinel/run.sh" ]
