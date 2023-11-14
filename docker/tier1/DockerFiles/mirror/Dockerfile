# syntax = docker/dockerfile:1.4

FROM archlinux:latest

# Install base packages for the mirror
RUN pacman -Syu --noconfirm nginx

ENTRYPOINT /usr/bin/nginx