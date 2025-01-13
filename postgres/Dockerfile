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

# Use the 'postgres' user to run the process
USER postgres

# Expose PostgreSQL port
EXPOSE 5432

# Initialize the PostgreSQL database
RUN /usr/pgsql-16/bin/initdb -D /var/lib/pgsql/16/data --encoding=UTF8

# Modify postgresql.conf to listen on all IPs
RUN echo "listen_addresses = '*'" >> /var/lib/pgsql/16/data/postgresql.conf

# Modify pg_hba.conf to allow connections from anywhere
RUN echo "host    all             all             172.18.0.0/16            md5" >> /var/lib/pgsql/16/data/pg_hba.conf

# Start PostgreSQL
CMD ["/usr/pgsql-16/bin/postgres", "-D", "/var/lib/pgsql/16/data"]
