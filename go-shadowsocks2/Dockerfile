FROM alpine:latest as downloader

RUN  wget -O - \
  https://github.com/shadowsocks/go-shadowsocks2/releases/download/v0.0.11/shadowsocks2-linux.gz  \
  | gunzip -c > /shadowsocks2 && \
  chmod +x /shadowsocks2

FROM scratch
COPY --from=downloader /shadowsocks2 /shadowsocks2

ENTRYPOINT [ "/shadowsocks2" ]