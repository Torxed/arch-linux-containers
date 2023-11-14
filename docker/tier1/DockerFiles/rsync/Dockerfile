# syntax = docker/dockerfile:1.4

FROM archlinux:latest

# Install base packages for the mirror
RUN pacman -Syu --noconfirm rsync

# Run rsyncd
ENTRYPOINT rsync -rlptH --safe-links --delete-delay --delay-updates --log-file=/dev/stdout rsync://$TIER0_ADDR:$TIER0_PORT/tier0 /srv/http/ || sleep 10