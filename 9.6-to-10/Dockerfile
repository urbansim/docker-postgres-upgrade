FROM postgres:10

RUN sed -i 's/$/ 9.6/' /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y --no-install-recommends \
		postgresql-9.6=9.6.9-2.pgdg90+1 \
		postgresql-contrib-9.6=9.6.9-2.pgdg90+1 \
	&& rm -rf /var/lib/apt/lists/*

RUN  apt-get update \
  && apt-get install -y --no-install-recommends \
        wget \
  && rm -rf /var/lib/apt/lists/*

RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt xenial-pgdg main" >> /etc/apt/sources.list'
RUN wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -
RUN apt update


RUN apt-get update && apt-get install -y --no-install-recommends \
        postgresql-10-postgis-2.4\
        postgresql-10-postgis-scripts\
        postgresql-10-pgrouting	\
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y --no-install-recommends \
        postgresql-9.6-postgis-2.4\
        postgresql-9.6-postgis-scripts\
        postgresql-9.6-pgrouting	\
    && rm -rf /var/lib/apt/lists/*

ENV PGBINOLD /usr/lib/postgresql/9.6/bin
ENV PGBINNEW /usr/lib/postgresql/10/bin

ENV PGDATAOLD /var/lib/postgresql/9.6/data
ENV PGDATANEW /var/lib/postgresql/10/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade /usr/local/bin/

ENTRYPOINT ["docker-upgrade"]

# recommended: --link
CMD ["pg_upgrade"]
