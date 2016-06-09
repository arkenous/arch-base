## -*- docker-image-name: "trileg/arch-base" -*-
FROM base/archlinux:latest
MAINTAINER Kensuke Kosaka <kosaka@trileg.net>

RUN pacman -Syy --noconfirm > /dev/null
RUN pacman -S archlinux-keyring --noconfirm > /dev/null
RUN pacman -Syu --noconfirm &> /dev/null
RUN ln -s /usr/lib/libncursesw.so.6 /usr/lib/libncursesw.so.5
RUN pacman-db-upgrade > /dev/null
RUN trust extract-compat

# Install yaourt
RUN pacman -Syu --noconfirm patch > /dev/null
COPY patches/pacman.conf.patch /tmp/pacman.conf.patch
RUN patch -u /etc/pacman.conf < /tmp/pacman.conf.patch && rm -f /tmp/pacman.conf.patch
RUN pacman -Syu yaourt --noconfirm > /dev/null

# Add normal user who can escalate to root without a password
RUN pacman -Syu --noconfirm sudo > /dev/null
COPY patches/sudoers.patch /tmp/sudoers.patch
RUN patch -u /etc/sudoers < /tmp/sudoers.patch && rm -f /tmp/sudoers.patch
RUN chown root:root /etc/sudoers && chmod 440 /etc/sudoers
RUN pacman -Syu --noconfirm util-linux &> /dev/null
RUN useradd -m -d /home/user -s /bin/bash -g users -G users,wheel user

RUN pacman -Scc --noconfirm > /dev/null
ONBUILD RUN pacman -Syu --noconfirm &> /dev/null
