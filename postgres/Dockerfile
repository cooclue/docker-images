# ⚠️ 基础镜像须与 abcfy2/zhparser:18 同为 trixie/PG18.4，否则 COPY 来的 .so ABI 不匹配
FROM postgres:18.4

COPY --from=abcfy2/zhparser:18 /usr/lib/postgresql/18/lib/zhparser.so               /usr/lib/postgresql/18/lib/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/extension/zhparser.control  /usr/share/postgresql/18/extension/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/extension/zhparser--2.3.sql /usr/share/postgresql/18/extension/
COPY --from=abcfy2/zhparser:18 /usr/local/lib/libscws.so.1                          /usr/local/lib/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/tsearch_data/               /usr/share/postgresql/18/tsearch_data/

RUN ldconfig && apt-get update && apt-get install -y --no-install-recommends \
      postgresql-18-pgvector \
      postgresql-18-cron \
      postgresql-18-repack \
      postgresql-18-hypopg \
      postgresql-18-hll \
 && rm -rf /var/lib/apt/lists/*

# pg_textsearch 无 PGDG 包，取官方预编译包
ADD https://github.com/timescale/pg_textsearch/releases/download/v1.3.1/pg_textsearch-pg18-v1.3.1.tar.gz /tmp/pgts.tgz
RUN tar xzf /tmp/pgts.tgz -C /tmp \
 && install -m 0755 /tmp/pg18/pg_textsearch.so /usr/lib/postgresql/18/lib/ \
 && install -m 0644 /tmp/pg18/pg_textsearch.control /tmp/pg18/pg_textsearch--*.sql /usr/share/postgresql/18/extension/ \
 && rm -rf /tmp/pg18 /tmp/pgts.tgz
