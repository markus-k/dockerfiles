FROM debian

ARG SIA_VERSION=1.4.1.2

RUN apt-get update && \
    apt-get install -y wget unzip socat && \
    mkdir -p /sia && \
    wget -O /tmp/sia.zip https://sia.tech/releases/Sia-v$SIA_VERSION-linux-amd64.zip && \
    wget -O /tmp/sia.pem https://sia.tech/releases/sia.pem && \
    unzip -d /tmp/sia/ /tmp/sia.zip && \
    openssl dgst -sha256 -verify /tmp/sia.pem -signature /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siac.sig /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siac && \
    openssl dgst -sha256 -verify /tmp/sia.pem -signature /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siad.sig /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siad && \
    mv /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siac /usr/local/bin/ && \
    mv /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siad /usr/local/bin/ && \
    rm -rf /tmp/sia/ /tmp/sia.zip /tmp/sia.pem /var/lib/apt/lists

WORKDIR /sia
EXPOSE 9981 9982

# set "-e EXPOSE_INTERNAL" when calling "docker run" to bind the internal port to 8000
# this is for testing only and is considered harmful
CMD if test "$EXPOSE_INTERNAL"; then socat tcp-listen:8000,reuseaddr,fork tcp:localhost:9980 & fi; /usr/local/bin/siad
