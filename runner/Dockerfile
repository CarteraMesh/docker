FROM  ghcr.io/actions/actions-runner:latest
USER root
RUN apt-get update -y \
  && apt-get install -y curl gnupg git procps psutils clang cmake build-essential pkg-config libssl-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER runner
ENV PATH=/home/runner/.cargo/bin:$PATH
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
RUN rustup toolchain list
RUN cargo install cargo-llvm-cov
RUN rustup install beta && rustup install nightly
RUN rustc --version
