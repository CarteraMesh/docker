FROM ubuntu:24.04 AS builder

ARG UID=101
ARG GID=101

RUN groupadd --gid ${GID} bitcoin \
  && useradd --create-home --no-log-init -u ${UID} -g ${GID} bitcoin \
  && apt-get update -y \
  && apt-get install -y curl gnupg gosu git procps psutils clang cmake build-essential pkg-config libssl-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV TARGETPLATFORM=x86_64-linux-gnu
ENV BITCOIN_VERSION=29.0
ENV BITCOIN_DATA=/home/bitcoin/.bitcoin


RUN set -ex \
  && if [ "${TARGETPLATFORM}" = "linux/amd64" ]; then export TARGETPLATFORM=x86_64-linux-gnu; fi \
  && if [ "${TARGETPLATFORM}" = "linux/arm64" ]; then export TARGETPLATFORM=aarch64-linux-gnu; fi \
  && if [ "${TARGETPLATFORM}" = "linux/arm/v7" ]; then export TARGETPLATFORM=arm-linux-gnueabihf; fi \
  && git clone --depth 1  https://github.com/bitcoin-core/guix.sigs \
  && gpg --batch --import guix.sigs/builder-keys/* \
  && curl -SLO https://bitcoincore.org/bin/bitcoin-core-${BITCOIN_VERSION}/bitcoin-${BITCOIN_VERSION}-${TARGETPLATFORM}.tar.gz \
  && curl -SLO https://bitcoincore.org/bin/bitcoin-core-${BITCOIN_VERSION}/SHA256SUMS \
  && curl -SLO https://bitcoincore.org/bin/bitcoin-core-${BITCOIN_VERSION}/SHA256SUMS.asc \
  && gpg --verify SHA256SUMS.asc SHA256SUMS \
  && grep " bitcoin-${BITCOIN_VERSION}-${TARGETPLATFORM}.tar.gz" SHA256SUMS | sha256sum -c - \
  && tar -xzf *.tar.gz -C /opt \
  && rm *.tar.gz *.asc \
  && rm -rf /opt/bitcoin-${BITCOIN_VERSION}/bin/bitcoin-qt \
  && mv -v /opt/bitcoin-*/bin/* /usr/bin/
   
COPY rustup.sh /usr/local/bin/rustup.sh
COPY ord-install.sh /usr/local/bin/ord-install.sh
RUN rustup.sh -y
RUN . $HOME/.cargo/env && cargo install electrs
RUN . $HOME/.cargo/env && ord-install.sh --to /usr/bin
COPY docker-entrypoint.sh /entrypoint.sh

FROM ubuntu:24.04

ARG UID=101
ARG GID=101

ENV TINI_VERSION=v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]

COPY --from=builder /entrypoint.sh /entrypoint.sh
COPY --from=builder /usr/bin/bitc* /usr/bin/
COPY --from=builder /root/.cargo/bin/electrs /usr/bin/electrs
#COPY --from=builder /root/.cargo/bin/bitcoin-exporter /usr/bin/bitcoin-exporter
COPY --from=builder /usr/bin/ord /usr/bin/ord

RUN groupadd --gid ${GID} bitcoin \
  && useradd --create-home --no-log-init -u ${UID} -g ${GID} bitcoin \
  && apt-get update -y \
  && apt-get install -y curl  \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && chown -R bitcoin:bitcoin /home/bitcoin \
  && bitcoind -version

VOLUME ["/var/lib/bitcoind"]
EXPOSE 8332 8333 18332 18333 18443 18444 38333 38332
#ENTRYPOINT ["/entrypoint.sh"]
USER bitcoin
RUN bitcoind -version
RUN ord --version
RUN electrs --version
