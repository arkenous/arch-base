## -*- docker-image-name: "trileg/arch-base" -*-
FROM archlinuxjp/archlinux:latest
MAINTAINER Kensuke Kosaka <k@trileg.net>

RUN pacman -Syy --noconfirm > /dev/null

# Add normal user who can escalate to root without a password
COPY patches/sudoers.patch /tmp/sudoers.patch
RUN pacman -Syu --noconfirm base-devel &> /dev/null && \
  patch -u /etc/sudoers < /tmp/sudoers.patch && rm -f /tmp/sudoers.patch && \
  chown root:root /etc/sudoers && chmod 440 /etc/sudoers && \
  useradd -m -d /home/user -s /bin/bash -g users -G users,wheel user

USER user

# INSTALL pacaur
## Install git
RUN sudo pacman -Syu --noconfirm git > /dev/null

## Install cower
RUN curl -L -o /tmp/cower.tar.gz --silent https://aur.archlinux.org/cgit/aur.git/snapshot/cower.tar.gz && \
  tar -zxvf /tmp/cower.tar.gz -C /tmp/ > /dev/null && \
  cd /tmp/cower && \
  gpg --recv-keys 1EB2638FF56C0C53 2> /dev/null && \
  export PATH=/usr/bin/core_perl:$PATH && \
  makepkg -sri --noconfirm &> /dev/null && \
  rm -rf /tmp/cower* && \
  cd /

## Install expac
RUN sudo pacman -Syu --noconfirm expac > /dev/null

# Install pacaur
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
