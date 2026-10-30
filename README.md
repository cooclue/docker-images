# docker-images

定制容器镜像，构建产物推 `ghcr.io/cooclue/*`。

| 镜像 | 内容 |
|---|---|
| `postgres` | PostgreSQL 18.6 + zhparser（中文分词）+ pgvector / pg_textsearch / hll / pg_cron / pg_repack / hypopg，`linux/amd64` + `linux/arm64` |
| `caddy` | Caddy 2.11.7 + 华为云 DNS 插件（`caddy-dns/huaweicloud`，DNS-01 签泛证书）+ 限流插件（`mholt/caddy-ratelimit`，按客户端 IP 计数），`linux/amd64` + `linux/arm64` |

改某个镜像目录下的文件推 main，只构建并推送该镜像（每个镜像一个 workflow，按路径触发）。

⚠️ **改内容就递增 tag**（`18.4-ext1` → `18.4-ext2`，`2.11.7-hwdns1` → `2.11.7-hwdns2`；tag 写在该镜像的 workflow 里），别覆盖旧 tag：消费方按 tag 引用，覆盖会让不同机器同 tag 拿到不同镜像。

```bash
docker pull ghcr.io/cooclue/postgres:18.6-ext1
```
