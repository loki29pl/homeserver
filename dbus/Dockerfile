FROM debian:jessie

RUN apt-get update
RUN apt-get install -y dbus

ADD session.conf /etc/dbus-1/session.conf
ADD system.conf /etc/dbus-1/system.conf

RUN mkdir -p /var/run/dbus/

EXPOSE 34350
EXPOSE 34351

CMD rm -rf /var/run/dbus/pid && dbus-daemon --system && dbus-daemon --session