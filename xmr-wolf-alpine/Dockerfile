FROM alpine:3.4

RUN set -x \
    # Runtime dependencies.
 && apk add --no-cache \
        bash \
        libcurl \
        libgcc \
        libstdc++ \
        openssl \
    # Build dependencies.
 && apk add --no-cache -t .build-deps \
        autoconf \
        automake \
        build-base \
        curl \
        curl-dev \
        git \
        openssl-dev \
    # Compile from source code.
 && git clone https://github.com/wolf9466/cpuminer-multi.git /tmp/cpuminer \
 && cd /tmp/cpuminer \
 && ./autogen.sh \
 && ./configure CFLAGS="-march=native" \
 && make install \
    # Install dumb-init (avoid PID 1 issues).
    # https://github.com/Yelp/dumb-init
 && curl -Lo /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.1.3/dumb-init_1.1.3_amd64 \
 && chmod +x /usr/local/bin/dumb-init \
    # Clean-up
 && cd / \
 && apk del --purge .build-deps \
 && rm -rf /tmp/* \
    # Verify
 && minerd --version

ADD start.sh /start.sh
RUN chmod 755 /start.sh

ENV PASSWORD=x
ENV URL="stratum+tcp://xmr.crypto-pool.fr:3333"
ENV ALGORITHM=cryptonight
ENV PRIORITY=19

ENTRYPOINT ["dumb-init"]
CMD ["/start.sh"]
