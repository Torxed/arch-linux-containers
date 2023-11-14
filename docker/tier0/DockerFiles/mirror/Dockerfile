# syntax = docker/dockerfile:1.4

FROM archlinux:latest

# Install base packages for the mirror
RUN pacman -Syu --noconfirm nginx

# Create a build user for 'makepkg'
RUN useradd --no-create-home --shell=/bin/nologin builder && usermod -L builder
# We don't need sudo if we use pacman for dependency installation
# RUN echo "builder ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/builder

COPY ./packages/dummy1 /build/dummy1
WORKDIR /build/dummy1

# Install dependencies as makepkg has troubles doing it
RUN source /build/dummy1/PKGBUILD && pacman -S --noconfirm --needed --asdeps "${makedepends[@]}" "${depends[@]}"

# Build the package
RUN chown builder:builder -R /build
USER builder
RUN makepkg -s -f --clean --cleanbuild --nodeps --nocolor

USER root

# Move the package to the mirror location
RUN mkdir -p /srv/http/core/os/x86_64/
RUN mv *.zst /srv/http/core/os/x86_64/
RUN rm /etc/sudoers.d/builder || exit 0
RUN userdel builder || exit 0

RUN repo-add --new --remove /srv/http/core/os/x86_64/core.db.tar.gz /srv/http/core/os/x86_64/*.zst

RUN chown -R http:http /srv/http
WORKDIR /srv/http

USER root
ENTRYPOINT /usr/bin/nginx