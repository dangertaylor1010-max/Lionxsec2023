FROM debian:bookworm-slim

COPY install.sh /app/install.sh

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    bash git curl wget unzip tzdata openssl ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN chmod +x /app/install.sh && /app/install.sh

COPY config.json /etc/config.json

# Generate a unique random UUID and write it into config
RUN UUID=$(cat /proc/sys/kernel/random/uuid) && \
    sed -i "s/XRAY_UUID_PLACEHOLDER/$UUID/g" /etc/config.json

# Show correct VLESS link using Codespace hostname (not hardcoded IP)
RUN echo '\
UUID=$(grep -o '"'"'"id": "[^"]*"'"'"' /etc/config.json | head -1 | cut -d'"'"'"'"'"' -f4)\n\
echo ""\n\
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"\n\
echo "✅  LionXsec Tunnel آماده اتصال است"\n\
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"\n\
echo ""\n\
echo "🔗 لینک VLESS:"\n\
echo "vless://${UUID}@${CODESPACE_NAME}-443.app.github.dev:443?encryption=none&security=tls&type=xhttp&mode=packet-up&sni=${CODESPACE_NAME}-443.app.github.dev&path=%2F#LionXsec"\n\
echo ""\n\
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"\n\
' >> /etc/bash.bashrc

CMD ["/usr/local/bin/xray", "-c", "/etc/config.json"]
