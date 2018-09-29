FROM alpine:3.8
LABEL Maintainer="Scott Bardua <sbardua@gmail.com>" \
      Description="Lightweight Mosquitto MQTT broker based on Alpine Linux."

ARG UID=1000
ARG GID=1000
ARG USERNAME=mosquitto
ARG PASSWORD=notsecure

RUN set -xe \
    && addgroup -g ${GID} -S mosquitto \
    && adduser -u ${UID} -G mosquitto -h /mosquitto/ -D mosquitto \
    && apk update \
    && apk add --no-cache --purge -uU ca-certificates mosquitto mosquitto-libs mosquitto-clients \
    && rm -rf /var/cache/apk/* /tmp/* \
    && mkdir -p /mosquitto/config \
    && mkdir -p /mosquitto/data \
    && mkdir -p /mosquitto/log \
    && touch /mosquitto/config/pwfile \
    && mosquitto_passwd -b /mosquitto/config/pwfile "${USERNAME}" "${PASSWORD}" \
    && chown -R mosquitto:mosquitto /mosquitto/

COPY --chown=mosquitto:mosquitto config/mosquitto.conf /mosquitto/config/mosquitto.conf

EXPOSE 1883 9001

ENTRYPOINT ["mosquitto"]
CMD ["-c", "/mosquitto/config/mosquitto.conf"]
