FROM hkgts/che-centos:7.4.1708

ARG MYSQL_SERVER_PACKAGE_URL=https://repo.mysql.com/yum/mysql-5.6-community/docker/x86_64/mysql-community-server-minimal-5.6.41-2.el7.x86_64.rpm
ARG MYSQL_SHELL_PACKAGE_URL=

# Install server
RUN sudo rpmkeys --import https://repo.mysql.com/RPM-GPG-KEY-mysql \
  && sudo yum install -y $MYSQL_SERVER_PACKAGE_URL $MYSQL_SHELL_PACKAGE_URL libpwquality \
  && sudo yum clean all \
  && sudo mkdir /docker-entrypoint-initdb.d

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /entrypoint.sh
COPY healthcheck.sh /healthcheck.sh
ENTRYPOINT ["/entrypoint.sh"]
HEALTHCHECK CMD /healthcheck.sh
EXPOSE 3306
CMD ["mysqld"]
