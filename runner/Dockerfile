FROM ghcr.io/actions/actions-runner:latest
ENV PATH=/home/runner/.bun/bin/:/home/runner/externals/node24/bin:$PATH
ENV FORCE_JAVASCRIPT_ACTIONS_TO_NODE24=true
COPY bun.sh .
RUN bash bun.sh && bun --version && node --version
