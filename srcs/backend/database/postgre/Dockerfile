# Usa la imagen oficial de PostgreSQL
FROM postgres:13

# Instala OpenSSL y wget
RUN apt-get update && apt-get install -y wget && rm -rf /var/lib/apt/lists/*

# Descarga y extrae el postgres-exporter
RUN wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.15.0/postgres_exporter-0.15.0.linux-amd64.tar.gz \
    -O /tmp/postgres-exporter.tar.gz && \
    tar -zxvf /tmp/postgres-exporter.tar.gz -C /tmp && \
    mv /tmp/postgres_exporter-0.15.0.linux-amd64/postgres_exporter /usr/local/bin/postgres-exporter && \
    rm -rf /tmp/postgres-exporter.tar.gz /tmp/postgres_exporter-0.15.0.linux-amd64

# Da permisos de ejecución al exporter
RUN chmod +x /usr/local/bin/postgres-exporter

ARG PASSWORD
# Variables para PostgreSQL
ENV POSTGRES_DB=transcendence
ENV POSTGRES_USER=admin
ENV POSTGRES_PASSWORD="${PASSWORD}"
# Variable para el Exporter
ENV DATA_SOURCE_NAME="postgresql://admin:${PASSWORD}@localhost:5432/transcendence?sslmode=disable"

# Ejecuta PostgreSQL y el exporter
CMD ["sh", "-c", "/usr/local/bin/postgres-exporter & docker-entrypoint.sh postgres > /var/log/postgresql/postgresql.log 2>&1"]

# PostgreSQL & Exporter
EXPOSE 5432 9187
