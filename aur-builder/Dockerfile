FROM archlinux:base-devel
RUN pacman -Sy && pacman-key --init &&  pacman -Su --noconfirm --needed nvchecker git curl wget namcap pacman-contrib jq && rm -rf /var/cache/pacman/pkg
COPY pkgbuild-nvchecker /usr/local/bin/pkgbuild-nvchecker
# match github runner uid
RUN groupadd --gid 1001 aur_builder &&  useradd --uid 1001 --gid 1001 --no-user-group --create-home --shell /bin/bash aur_builder
USER aur_builder
WORKDIR /home/aur_builder
RUN mkdir .ssh && git config --global user.name aur-builder && git config --global user.email aur-builder@bot.com
COPY known_hosts .ssh/known_hosts
RUN makepkg --version && nvchecker --version && jq --version && namcap --version && updpkgsums --version
