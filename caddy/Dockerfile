# Caddy + 华为云 DNS 插件（DNS-01 签泛证书）；Go 交叉编译，builder 不走 qemu
FROM --platform=$BUILDPLATFORM caddy:2.11.7-builder AS builder
ARG TARGETOS TARGETARCH
RUN GOOS=$TARGETOS GOARCH=$TARGETARCH xcaddy build \
    --with github.com/caddy-dns/huaweicloud@v1.0.1

FROM caddy:2.11.7
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
