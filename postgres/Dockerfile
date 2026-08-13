# ⚠️ 基础镜像须与 abcfy2/zhparser:18 同为 trixie/PG18.4，否则 COPY 来的 .so ABI 不匹配
FROM postgres:18.4
ARG TARGETARCH

COPY --from=abcfy2/zhparser:18 /usr/lib/postgresql/18/lib/zhparser.so               /usr/lib/postgresql/18/lib/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/extension/zhparser.control  /usr/share/postgresql/18/extension/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/extension/zhparser--2.3.sql /usr/share/postgresql/18/extension/
COPY --from=abcfy2/zhparser:18 /usr/local/lib/libscws.so.1                          /usr/local/lib/
COPY --from=abcfy2/zhparser:18 /usr/share/postgresql/18/tsearch_data/               /usr/share/postgresql/18/tsearch_data/

# pg_textsearch 无 PGDG 包，取官方分架构 .deb（zip 套一层）
ADD https://github.com/timescale/pg_textsearch/releases/download/v1.3.1/pg-textsearch-v1.3.1-pg18-${TARGETARCH}.zip /tmp/pgts.zip

RUN ldconfig && apt-get update && apt-get install -y --no-install-recommends \
      postgresql-18-pgvector \
      postgresql-18-cron \
      postgresql-18-repack \
      postgresql-18-hypopg \
      postgresql-18-hll \
      unzip \
 && unzip /tmp/pgts.zip -d /tmp \
 && apt-get install -y /tmp/pg-textsearch-postgresql-18_*.deb \
 && rm -rf /var/lib/apt/lists/* /tmp/pgts.zip /tmp/pg-textsearch-postgresql-18_*.deb
