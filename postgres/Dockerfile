# ⚠️ 基础镜像须与 zhparser 镜像同为 trixie/PG18.6，否则 COPY 来的 .so ABI 不匹配；zhparser:18 标签会随上游重推，故钉 digest，换 digest 时核对其 PG_VERSION
FROM abcfy2/zhparser:18@sha256:af1cd40f500a216fc5d2da7506212b0df6161451b5bbb368087177ab67c18a69 AS zhparser

FROM postgres:18.6
ARG TARGETARCH

COPY --from=zhparser /usr/lib/postgresql/18/lib/zhparser.so          /usr/lib/postgresql/18/lib/
COPY --from=zhparser /usr/share/postgresql/18/extension/zhparser*  /usr/share/postgresql/18/extension/
COPY --from=zhparser /usr/local/lib/libscws.so.1                     /usr/local/lib/
COPY --from=zhparser /usr/share/postgresql/18/tsearch_data/          /usr/share/postgresql/18/tsearch_data/

# pg_textsearch 无 PGDG 包，取官方分架构 .deb（zip 套一层）
ADD https://github.com/timescale/pg_textsearch/releases/download/v1.5.1/pg-textsearch-v1.5.1-pg18-${TARGETARCH}.zip /tmp/pgts.zip

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
