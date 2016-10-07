## -*- docker-image-name: "trileg/arch-base" -*-
FROM base/archlinux:latest
MAINTAINER Kensuke Kosaka <kosaka@trileg.net>

RUN pacman -Syy --noconfirm > /dev/null
RUN pacman -S archlinux-keyring --noconfirm > /dev/null
RUN pacman -Syu --noconfirm &> /dev/null
RUN ln -s /usr/lib/libncursesw.so.6 /usr/lib/libncursesw.so.5
RUN pacman-db-upgrade > /dev/null
RUN trust extract-compat


# Add normal user who can escalate to root without a password
RUN pacman -Syu --noconfirm base-devel &> /dev/null
COPY patches/sudoers.patch /tmp/sudoers.patch
RUN patch -u /etc/sudoers < /tmp/sudoers.patch && rm -f /tmp/sudoers.patch
RUN chown root:root /etc/sudoers && chmod 440 /etc/sudoers
RUN useradd -m -d /home/user -s /bin/bash -g users -G users,wheel user

# INSTALL pacaur
## Install git
RUN pacman -Syu --noconfirm git > /dev/null
## Install cower
USER user
RUN curl -L -o /tmp/cower.tar.gz --silent https://aur.archlinux.org/cgit/aur.git/snapshot/cower.tar.gz && \
  tar -zxvf /tmp/cower.tar.gz -C /tmp/ > /dev/null && \
  cd /tmp/cower && \
  gpg --recv-keys --keyserver hkp://pgp.mit.edu 1EB2638FF56C0C53 2> /dev/null && \
  export PATH=/usr/bin/core_perl:$PATH && \
  makepkg -sri --noconfirm &> /dev/null && \
  rm -rf /tmp/cower* && \
  cd /
USER root
## Install expac
RUN pacman -Syu --noconfirm expac > /dev/null
# Install pacaur
USER user
RUN curl -L -o /tmp/pacaur.tar.gz --silent https://aur.archlinux.org/cgit/aur.git/snapshot/pacaur.tar.gz && \
  tar -zxvf /tmp/pacaur.tar.gz -C /tmp/ > /dev/null && \
  cd /tmp/pacaur && \
  export PATH=/usr/bin/core_perl:$PATH && \
  makepkg -sri --noconfirm &> /dev/null && \
  rm -rf /tmp/pacaur* && \
  cd /
USER root


RUN pacman -Scc --noconfirm > /dev/null
ONBUILD RUN pacman -Syu --noconfirm &> /dev/null
