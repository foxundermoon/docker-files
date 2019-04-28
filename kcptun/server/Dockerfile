FROM alpine:latest as downloader
ARG VERSION=20190424

RUN mkdir /kcp && \
  wget -qO- \
  https://github.com/xtaci/kcptun/releases/download/v${VERSION}/kcptun-linux-amd64-${VERSION}.tar.gz \
  | tar xvz -C /kcp

FROM scratch
COPY --from=downloader /kcp/server_linux_amd64 /kcptun_server

ENTRYPOINT [ "/kcptun_server" ]