# Use the minimal base image
FROM redhat/ubi9-minimal:9.5

# Install dependencies
RUN microdnf install -y tar gzip dnf-utils && \
    microdnf clean all

# Add PostgreSQL repository
RUN curl -o /tmp/pgdg-redhat-repo-latest.noarch.rpm https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm && \
    rpm -i /tmp/pgdg-redhat-repo-latest.noarch.rpm && \
    rm /tmp/pgdg-redhat-repo-latest.noarch.rpm && \
    microdnf clean all

# Install PostgreSQL (e.g., version 16)
RUN microdnf install -y postgresql16-server && \
    microdnf clean all

# Asegurarse de que el directorio de datos esté limpio o inicializado
RUN mkdir -p /var/lib/pgsql/16/data && \
    chown -R postgres:postgres /var/lib/pgsql/16/data

# Usar el usuario 'postgres' para ejecutar el proceso
USER postgres

# Expose PostgreSQL port
EXPOSE 5432

RUN /usr/pgsql-16/bin/initdb -D /var/lib/pgsql/16/data && \
    /usr/pgsql-16/bin/pg_ctl start -D /var/lib/pgsql/16/data 

CMD ["/usr/pgsql-16/bin/postgres", "-D", "/var/lib/pgsql/16/data"]

