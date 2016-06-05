## -*- docker-image-name: "trileg/arch-base" -*-
FROM base/archlinux:latest
MAINTAINER Kensuke Kosaka <kosaka@trileg.net>

RUN pacman -Syy --noconfirm > /dev/null
RUN pacman -S archlinux-keyring --noconfirm > /dev/null
RUN pacman -Syu --noconfirm &> /dev/null
RUN ln -s /usr/lib/libncursesw.so.6 /usr/lib/libncursesw.so.5
RUN pacman-db-upgrade > /dev/null
RUN trust extract-compat
RUN pacman -Scc --noconfirm > /dev/null

ONBUILD RUN pacman -Syu --noconfirm > /dev/null
