FROM "local/ldap-template"

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN chmod 0755 /var/run/sshd

EXPOSE 22

RUN apt-get install -y \
    vim \
    traceroute \
    mtr \
    nmap \
    wget \
    curl \
    dnsutils

CMD nslcd; /usr/sbin/sshd -D