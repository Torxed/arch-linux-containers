# syntax = docker/dockerfile:1.4

FROM archlinux:latest

# Install base packages for the mirror
RUN pacman -Syu --noconfirm rsync

# Run rsyncd
ENTRYPOINT rsync --daemon --no-detach --config=/etc/rsync/rsyncd.conf --log-file=/dev/stdout