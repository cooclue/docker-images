# docker-images

定制容器镜像，构建产物推 `ghcr.io/cooclue/*`。

| 镜像 | 内容 |
|---|---|
| `postgres` | PostgreSQL 18.6 + zhparser（中文分词）+ pgvector / pg_textsearch / hll / pg_cron / pg_repack / hypopg，`linux/amd64` + `linux/arm64` |

改 `postgres/Dockerfile` 推 main 即自动构建并推送。

⚠️ **改内容就递增 tag**（`18.4-ext1` → `18.4-ext2`），别覆盖旧 tag：消费方按 tag 引用，覆盖会让不同机器同 tag 拿到不同镜像。

```bash
docker pull ghcr.io/cooclue/postgres:18.6-ext1
```
