FROM debian:jessie

ENV DBUS_SYSTEM_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4
ENV DBUS_SESSION_BUS_ADDRESS tcp:host=10.121.0.10,bind=*,port=34350,family=ipv4

RUN apt-get update
RUN apt-get install -y libavahi-compat-libdnssd-dev \
        psmisc \
        avahi-daemon \
        avahi-discover \
        libnss-mdns \
        avahi-utils

EXPOSE 5353/udp

CMD pkill -F var/run/avahi-daemon//pid

CMD avahi-daemon