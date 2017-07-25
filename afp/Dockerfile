FROM "local/ldap-template"

ENV DBUS_SYSTEM_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4
ENV DBUS_SESSION_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4

RUN apt-get update

RUN apt-get install -y \
    wget \
    make \
    gcc \
    bzip2 \
    libtool \
    autoconf \
    libgcrypt20-dev \
    libdb-dev \
    libldap2-dev \
    libavahi-client-dev \
    libcrack2-dev \
    libpam0g-dev

RUN mkdir /opt/netatalk
WORKDIR /opt/netatalk

RUN wget "https://downloads.sourceforge.net/project/netatalk/netatalk/3.1.11/netatalk-3.1.11.tar.bz2?r=http%3A%2F%2Fnetatalk.sourceforge.net%2F&ts=1500919096&use_mirror=kent" -O netatalk.tar.bz2

RUN tar -xjf netatalk.tar.bz2
WORKDIR /opt/netatalk/netatalk-3.1.11

RUN ./configure \
    --with-ldap \
    --with-cracklib \
    --enable-quota \
    --with-pam \
    --without-shadow

RUN make
RUN make install

EXPOSE 548

CMD nslcd; cnid_metad; afpd -d