ARG BUILD_FROM=ghcr.io/erik73/base:6.0.6
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache \
        mariadb-client=11.4.9-r0 \
        nginx=1.28.2-r1 \
        php85-curl=8.5.2-r0 \
        php85-fpm=8.5.2-r0 \
        php85-gd=8.5.2-r0 \
        php85-iconv=8.5.2-r0 \
        php85-mbstring=8.5.2-r0 \
        php85-mysqli=8.5.2-r0 \
        php85-openssl=8.5.2-r0 \
        php85-session=8.5.2-r0 \
        php85-xml=8.5.2-r0 \
        php85-zip=8.5.2-r0 \
        php85=8.5.2-r0 \
    \
    && mkdir /var/www/phpmyadmin \
    && curl -L -s \
        "https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz" \
        | tar zxvf - --strip 1 -C /var/www/phpmyadmin \
    \
    && sed -i "s@'configFile' =>.*@'configFile' => '/etc/phpmyadmin/config.inc.php',@" \
        /var/www/phpmyadmin/libraries/vendor_config.php \
    \
    && find /var/www/ -type f -name "*.md" -depth -exec rm -f {} \; \
    && rm -f -r \
        /tmp/* \
        /etc/nginx \
        /var/www/phpmyadmin/setup \
        /var/www/phpmyadmin/examples \
        /var/www/phpmyadmin/test/ \
        /var/www/phpmyadmin/po \
        /var/www/phpmyadmin/composer.json \
        /var/www/phpmyadmin/RELEASE-DATE-*

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Erik Hilton <erikhilton73@gmail.com>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Erik73 Hassio-Addons" \
    org.opencontainers.image.authors="Erik Hilton <erikhilton73@gmail.com>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/erik73/hassio-addons" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
