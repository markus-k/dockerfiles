FROM debian

ARG SIA_VERSION=1.5.0

RUN apt-get update && \
    apt-get install -y wget unzip gpg && \
    mkdir -p /sia && \
    wget -O /tmp/sia.zip https://sia.tech/releases/Sia-v$SIA_VERSION-linux-amd64.zip && \
    wget -O /tmp/Sia-SHA256SUMS.txt.asc https://sia.tech/releases/Sia-v$SIA_VERSION-SHA256SUMS.txt.asc && \
    wget -O /tmp/sia-signing-key.asc https://sia.tech/releases/sia-signing-key.asc && \
    gpg --import /tmp/sia-signing-key.asc && \
    gpg --verify /tmp/Sia-SHA256SUMS.txt.asc && \
    unzip -d /tmp/sia/ /tmp/sia.zip && \
    cd /tmp/sia/ && sha256sum --check --ignore-missing /tmp/Sia-SHA256SUMS.txt.asc && cd / && \
    mv /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siac /usr/local/bin/ && \
    mv /tmp/sia/Sia-v$SIA_VERSION-linux-amd64/siad /usr/local/bin/ && \
    rm -rf /tmp/sia/ /tmp/sia.zip /tmp/sia-signing-key.asc /tmp/Sia-SHA256SUMS.txt.asc /var/lib/apt/lists

WORKDIR /sia
EXPOSE 9981 9982

CMD ["/usr/local/bin/siad"]
