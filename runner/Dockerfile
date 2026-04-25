FROM ghcr.io/actions/actions-runner:latest
LABEL DAGGER_VERSION=v0.20.6
ENV DAGGER_VERSION=v0.20.6
USER root
COPY dagger.sh /tmp/
RUN apt-get update -y \
  && apt-get install -y curl nvchecker \
  && apt-get clean \
  && BIN_DIR=/usr/local/bin bash /tmp/dagger.sh && rm /tmp/dagger.sh \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER runner
ENV PATH=/home/runner/.bun/bin/:/home/runner/externals/node24/bin:$PATH
ENV FORCE_JAVASCRIPT_ACTIONS_TO_NODE24=true
COPY bun.sh .
RUN bash bun.sh && rm bun.sh &&  bun --version && node --version && nvchecker --version && dagger version
