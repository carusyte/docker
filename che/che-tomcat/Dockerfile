FROM hkgts/che-centos:7.4.1708

MAINTAINER carusyte <carusyte@163.com>

EXPOSE 4403 8080 8000 9876 22

LABEL che:server:8080:ref=tomcat7 che:server:8080:protocol=http che:server:8000:ref=tomcat7-debug che:server:8000:protocol=http che:server:9876:ref=codeserver che:server:9876:protocol=http

ARG JAVA_VERSION=1.8.0
ARG JAVA_PKG=java-1.8.0-openjdk-devel-1:1.8.0.181-3.b13.el7_5.x86_64
# TOMCAT_VERSION=7.0.84

ENV JAVA_HOME=/usr/lib/jvm/java-${JAVA_VERSION}
ENV M2_HOME=/opt/rh/rh-maven33/root/usr/share/maven \
    TOMCAT_HOME=/home/user/tomcat7 \
    TERM=xterm
ENV PATH=$JAVA_HOME/bin:$M2_HOME/bin:$PATH

RUN sudo yum -y update && \
    sudo yum -y install \
    ${JAVA_PKG} rh-maven33 && \
    sudo yum clean all && \
    cat /opt/rh/rh-maven33/enable >> /home/user/.bashrc

USER user

ADD ./run.sh $HOME/run.sh

RUN mkdir -p $HOME/.m2 && \
    mkdir /home/user/tomcat7 && \
    wget -qO- "https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.84/bin/apache-tomcat-7.0.84.tar.gz" | tar -zx --strip-components=1 -C /home/user/tomcat7 && \
    rm -rf /home/user/tomcat7/webapps/* && \
    sudo chmod a+x $HOME/run.sh && \
    sudo mkdir -p /home/user/jdtls/data && \
    sudo chgrp -R 0 ${HOME} && \
    sudo chmod -R g+rwX ${HOME}

ADD ./settings.xml $HOME/.m2/settings.xml
RUN sudo chgrp -R 0 /home/user && \
    sudo chmod -R g+rwX /home/user


# override the default CMD form base image to allow configuring maven settings
CMD ["sh","-c","${HOME}/run.sh"]