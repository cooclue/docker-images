# Caddy + 华为云 DNS 插件（DNS-01 签泛证书）+ 按客户端 IP 限流插件；Go 交叉编译，builder 不走 qemu
FROM --platform=$BUILDPLATFORM caddy:2.11.7-builder AS builder
ARG TARGETOS TARGETARCH
RUN GOOS=$TARGETOS GOARCH=$TARGETARCH xcaddy build \
    --with github.com/caddy-dns/huaweicloud@v1.0.1 \
    --with github.com/mholt/caddy-ratelimit@5625512f24f6f59d6f64fb3aafe5eecff0b286db

FROM caddy:2.11.7
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
