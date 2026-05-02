FROM ubuntu:26.04
ENV DEBIAN_FRONTEND=noninteractive
ENV BUN_INSTALL=/usr/local
COPY *.sh /tmp/
RUN apt-get update -y && apt-get install -y curl zip \
  && curl -o /usr/share/keyrings/githubcli-archive-keyring.gpg -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg  \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main"  > /etc/apt/sources.list.d/github-cli.list \
  && apt-get update -y \
  && for i in /tmp/*.sh ; do bash $i ; done \
  && apt-get install -y gh git build-essential nvchecker jq python3 nodejs \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && node --version && npm --version && bun --version && gh version

COPY github-token-app /usr/local/bin/github-token-app



