FROM ghcr.io/actions/actions-runner:latest
USER root
RUN apt-get update -y \
  && apt-get install -y curl nvchecker \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER runner
ENV PATH=/home/runner/.bun/bin/:/home/runner/externals/node24/bin:$PATH
ENV FORCE_JAVASCRIPT_ACTIONS_TO_NODE24=true
COPY bun.sh .
RUN bash bun.sh && bun --version && node --version && nvchecker --version
