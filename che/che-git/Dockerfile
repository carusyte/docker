FROM alpine

LABEL maintainer="carusyte@163.com"

EXPOSE 22


RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
    shadow bash sudo git openssh openssh-server wget unzip bash-completion && \
    echo "%wheel ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 

# Adding user to the 'root' is a workaround for https://issues.jboss.org/browse/CDK-305
RUN adduser -D -u 1000 -h /home/user -s /bin/bash user
RUN usermod -a -G users,wheel,root -p "*" user
RUN sed -i 's/requiretty/!requiretty/g' /etc/sudoers

USER user
WORKDIR /projects

# The following instructions set the right
# permissions and scripts to allow the container
# to be run by an arbitrary user (i.e. a user
# that doesn't already exist in /etc/passwd)
ENV HOME /home/user
RUN for f in "/home/user" "/etc/passwd" "/etc/group" "/projects"; do\
    sudo chgrp -R 0 ${f} && \
    sudo chmod -R g+rwX ${f}; \
    done && \
    # Generate passwd.template \
    cat /etc/passwd | \
    sed s#user:x.*#user:x:\${USER_ID}:\${GROUP_ID}::\${HOME}:/bin/bash#g \
    > /home/user/passwd.template && \
    # Generate group.template \
    cat /etc/group | \
    sed s#root:x:0:#root:x:0:0,\${USER_ID}:#g \
    > /home/user/group.template && \
    sudo sed -ri 's/StrictModes yes/StrictModes no/g' /etc/ssh/sshd_config

COPY ["entrypoint.sh","/home/user/entrypoint.sh"]

ENTRYPOINT ["/home/user/entrypoint.sh"]
CMD tail -f /dev/null