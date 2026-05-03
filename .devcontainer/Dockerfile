FROM debian:bookworm-slim

COPY install.sh /app/install.sh

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    bash git curl wget unzip tzdata openssl ca-certificates qrencode \
    && rm -rf /var/lib/apt/lists/*

RUN chmod +x /app/install.sh && /app/install.sh
COPY config.json /etc/config.json

RUN cat > /etc/profile.d/ghtun.sh <<'EOF'
if [ -n "$PS1" ] && [ -z "$GHTUN_PRINTED" ]; then
  export GHTUN_PRINTED=1
  link="vless://550e8400-e29b-41d4-a716-446655440000@94.130.50.12:443?encryption=none&security=tls&type=xhttp&mode=packet-up&sni=${CODESPACE_NAME:-localhost}-443.app.github.dev&path=%2F#ghtun"
  echo
  echo "✅ VLESS LINK:"
  echo "$link"
  if command -v qrencode >/dev/null 2>&1; then
    echo
    echo "QR CODE:"
    qrencode -t ANSIUTF8 -o - "$link"
  fi
  echo
fi
EOF

CMD ["/usr/local/bin/xray", "-c", "/etc/config.json"]
