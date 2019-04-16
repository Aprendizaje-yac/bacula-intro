FROM ubuntu:bionic

LABEL maintainer="Backup Team PSI" \
      version="0.0.1"

# Para que no me tire bronca cuando instalo bacula con mysql
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update
RUN apt-get install -qqy --no-install-recommends bacula-common-mysql bacula-director bacula-director-mysql
RUN apt-get install -qqy --no-install-recommends bacula-console bacula-fd bacula-sd vim nano iputils-ping && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Change root password para entrar a webmin
RUN echo root:pass | chpasswd

RUN apt-get update
# Para administrar la DB desde webmin
RUN apt-get install -y libdbd-mysql-perl

#Seteamos la hora
#RUN echo America/Argentina/Cordoba >/etc/timezone && \
#    dpkg-reconfigure -f noninteractive tzdata

# Magia para webmin
RUN echo "Acquire::GzipIndexes \"false\"; Acquire::CompressionTypes::Order:: \"gz\";" >/etc/apt/apt.conf.d/docker-gzip-indexes
RUN apt-get install -y wget gnupg
# Add webmin repository key
RUN wget http://www.webmin.com/jcameron-key.asc && apt-key add jcameron-key.asc
# Add webmin repository
RUN echo "deb http://download.webmin.com/download/repository sarge contrib" >> /etc/apt/sources.list.d/webmin.list
# Update OS
RUN apt-get update && apt-get dist-upgrade -y
# Install webmin and clean file
RUN apt-get install -y webmin && apt-get autoclean

#Puerto para webmin
EXPOSE 10000
#Configuracion para webmin
COPY webmin.conf /etc/webmin/bacula-backup/config
#Configuracion de bacula
#COPY bacula-dir.conf /etc/bacula/bacula-dir.conf
#COPY bconsole.conf /etc/bacula/bconsole.conf
#COPY bacula-fd.conf /etc/bacula/bacula-fd.conf
#COPY bacula-sd.conf /etc/bacula/bacula-sd.conf
#COPY up-service.sh /services.sh
#COPY up-service.sh /up-services.sh

##################################
#Seteamos la hora
ENV TZ=America/Argentina/Cordoba
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#Instalamos SENDMAIL#############
#RUN apt-get install -y sendmail-bin sendmail sendmail-cf
#ADD mail /etc/
#COPY baculareport.sh /root/tools/baculareport.sh
#RUN ln -s -f /etc/mail/aliases /etc/aliases
#COPY aliases /etc/aliases
#RUN apt-get install -y gawk

##################################

#RUN mkdir -p /mnt/backup-1 && chown -R bacula:bacula /mnt/backup-1
#RUN mkdir -p /mnt/backup-2 && chown -R bacula:bacula /mnt/backup-2
#RUN mkdir -p /mnt/backup-3 && chown -R bacula:bacula /mnt/backup-3

#RUN chown -R bacula:bacula /etc/bacula
#RUN mkdir -p /var/lib/bacula && chown -R bacula:bacula /var/lib/bacula
#RUN mkdir -p /var/run/bacula && chown -R bacula:bacula /var/run/bacula
#RUN mkdir -p /run/bacula && chown -R bacula:bacula /run/bacula

#VOLUME ["/etc/bacula"]
#VOLUME ["/etc/webmin"]
#VOLUME ["/mnt/backup-1"]
#VOLUME ["/mnt/backup-2"]
#VOLUME ["/mnt/backup-3"]

#VOLUME ["/var/lib/bacula"]
##VOLUME ["/var/run/bacula"]
#VOLUME [ "/run/bacula" ]

#ENTRYPOINT [ "/usr/bin/touch /var/webmin/miniserv.log && /etc/init.d/webmin restart && /usr/bin/tail -f /var/webmin/miniserv.log" ]
ENTRYPOINT /usr/bin/touch /var/webmin/miniserv.log && /etc/init.d/webmin restart && /usr/bin/tail -f /var/webmin/miniserv.log  
