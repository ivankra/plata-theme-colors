# Builds a single Plata theme version from source.
# Produces tarballs and a debian package in container's /dist directory.

FROM debian:bullseye

SHELL ["/bin/bash", "-x", "-c"]

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        autoconf \
        automake \
        build-essential \
        ca-certificates \
        dpkg \
        fakeroot \
        git \
        gnome-shell \
        inkscape \
        libgdk-pixbuf2.0-dev \
        libgtk-3-dev \
        libglib2.0-dev \
        libmarco-dev \
        libxml2-utils \
        parallel \
        pkg-config \
        sassc \
        zip

RUN git clone https://gitlab.com/tista500/plata-theme.git /src

WORKDIR /src

ARG REVISION=master
RUN git reset --hard $REVISION

ARG FLAGS=
RUN ./autogen.sh \
        --prefix=/usr \
        --enable-parallel \
        --enable-gtk_next \
        --enable-gnome \
        --enable-cinnamon \
        --enable-flashback \
        --enable-xfce \
        --enable-mate \
        --enable-openbox \
        --enable-plank \
        --enable-telegram \
        --enable-tweetdeck \
        --enable-airforsteam \
        $FLAGS
RUN make -j
RUN make install

WORKDIR /usr/share/themes

# Rename built themes inside /usr/share/themes/
ARG THEME=Plata
RUN if [[ "$THEME" != "Plata" ]]; then \
        for x in Plata*; do \
            mv "$x" "$(echo "$x" | sed -e "s/^Plata/$THEME/")"; \
        done; \
    fi

RUN mkdir -p /dist

# Create tarballs
RUN for x in "$THEME"*; do tar -jcf "/dist/$x.tar.xz" "$x"; done

# Create debian package
RUN export PKG="${THEME,,}-gtk-theme" && \
    export VER="$(cd /src && git describe --tags)" && \
    mkdir -p /pkg/usr/share/themes && \
    cp -a "/usr/share/themes/$THEME"* /pkg/usr/share/themes/ && \
    mkdir -p "/pkg/usr/share/doc/$PKG" && \
    cp -a /src/COPYING /src/LICENSE* /src/README.md "/pkg/usr/share/doc/$PKG/" && \
    mkdir -p /pkg/DEBIAN && \
    ( \
        echo "Package: $PKG"; \
        echo "Version: $VER"; \
        echo "Architecture: all"; \
        echo "Maintainer: none"; \
        echo "Depends: gtk2-engines-murrine, gtk2-engines-pixbuf, fonts-roboto"; \
        echo "Section: x11"; \
        echo "Priority: optional"; \
        echo "Homepage: https://gitlab.com/tista500/plata-theme"; \
        echo "Description: ${THEME}, a Gtk theme based on Material Design Refresh."; \
    ) >/pkg/DEBIAN/control && \
    cat /pkg/DEBIAN/control && \
    fakeroot dpkg-deb --build -Zxz -z9 -Sextreme /pkg "/dist/${PKG}_${VER}_all.deb"
