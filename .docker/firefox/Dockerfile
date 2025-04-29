
ARG BASE_IMAGE=m1k1o/neko:latest
FROM --platform=linux/amd64 $BASE_IMAGE

ARG SRC_URL="https://download-installer.cdn.mozilla.net/pub/firefox/releases/138.0/linux-aarch64/en-GB/firefox-138.0.tar.xz"

# Install Firefox
RUN set -eux
RUN apt-get update
RUN apt-get install -y --no-install-recommends \
        wget \
        openbox \
        xz-utils \
        bzip2 \
        libgtk-3-0 \
        libdbus-glib-1-2


    
    # Fetch latest Firefox release (follow redirect)
RUN wget --trust-server-names -O /tmp/firefox-setup.tar.bz2 "$SRC_URL"

RUN ls -l /
RUN ls -l /tmp

RUN mkdir -p /usr/lib/firefox
RUN tar -xf /tmp/firefox-setup.tar.bz2 --strip-components=1 -C /usr/lib/firefox
RUN rm -f /tmp/firefox-setup.tar.bz2
RUN rm -rf /usr/bin/firefox
RUN ln -s /usr/lib/firefox/firefox /usr/bin/firefox

    # Create a profile directory
RUN mkdir -p /home/neko/.mozilla/firefox/profile.defaults/extensions
RUN chown -R neko:neko /home/neko/.mozilla/firefox/profile.defaults

    # Clean up
RUN apt-get --purge autoremove -y xz-utils bzip2
RUN apt-get clean -y
RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/*

# Copy configuration files
COPY supervisord.conf /etc/neko/supervisord/firefox.conf
COPY neko.js /usr/lib/firefox/mozilla.cfg
COPY autoconfig.js /usr/lib/firefox/defaults/pref/autoconfig.js
COPY policies.json /usr/lib/firefox/distribution/policies.json
COPY --chown=neko profiles.ini /home/neko/.mozilla/firefox/profiles.ini
COPY openbox.xml /etc/neko/openbox.xml
