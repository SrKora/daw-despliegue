FROM postgres:18-alpine

COPY escaparate/db/01-schema.sql escaparate/db/02-data.sql /docker-entrypoint-initdb.d/
