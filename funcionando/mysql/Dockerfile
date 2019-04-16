FROM mariadb:10.3

LABEL maintainer="Backup Team PSI" \
      version="0.0.1"

COPY make_mysql_tables_remote.sql /docker-entrypoint-initdb.d/make_mysql_tables_remote.sql
COPY .my.cnf /root/.my.cnf
COPY debian.cnf /etc/mysql/debian.cnf


##################################
#Seteamos la hora
ENV TZ=America/Argentina/Cordoba
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
