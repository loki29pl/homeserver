FROM debian:jessie

ENV DEBIAN_FRONTEND noninteractive
ENV DBUS_SYSTEM_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4
ENV DBUS_SESSION_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4

WORKDIR /root

# Wokraround for: Updates for this repository will not be applied.
RUN rm -Rf /var/lib/apt/

RUN apt-get update
RUN apt-get install -y wget \ 
        curl \
        apt-utils \
        git \
        g++ \
        make \
        libtool \
        automake \
        autoconf \
        libavahi-compat-libdnssd-dev \
        psmisc \
        libnss-mdns

RUN git clone --recursive https://github.com/obgm/libcoap.git
WORKDIR /root/libcoap
RUN git checkout dtls
RUN git submodule update --init --recursive
RUN ./autogen.sh
RUN ./configure --disable-documentation --disable-shared
RUN make
RUN make install 

WORKDIR /root

RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get update
RUN apt-get install -y nodejs

RUN npm install -g homebridge
RUN npm install -g homebridge-tradfri-plugin

RUN mkdir /root/.homebridge
ADD config.json /root/.homebridge/config.json

EXPOSE 51826

CMD homebridge
