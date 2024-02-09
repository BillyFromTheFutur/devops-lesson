FROM postgres:14.1-alpine
#Basic image of PostgreSQL database running on Alpine Linux

COPY init /docker-entrypoint-initdb.d
# Copies the contents of the  init directory to the container's entrypoint
# PostgreSQL executes scripts at the initialization, allowing the database to have a basic dump of data