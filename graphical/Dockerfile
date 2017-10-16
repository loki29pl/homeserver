FROM "local/ldap-template"

ENV DEBIAN_FRONTEND noninteractive

# Wokraround for: Updates for this repository will not be applied.
RUN rm -Rf /var/lib/apt/
RUN apt-get update

RUN apt-get install -y tightvncserver

RUN apt-get install -y \
    mate-session-manager \
    xrdp \
    lxde-core \
    vim

RUN apt-get install -y \
    iceweasel

WORKDIR /root
# Download from: http://www.nero.com/plk/downloads/previous-versions/download-linux4-update.php
ADD nerolinux.deb /root/nerolinux.deb
RUN dpkg -i nerolinux.deb

EXPOSE 3389

WORKDIR /etc/xrdp/
RUN xrdp-keygen xrdp

RUN mkdir /var/run/dbus

# nslcd;
# Must be set if you want access to burner
# setfacl -m g:burning:rwx /dev/sr0
# setfacl -m g:burning:rwx /dev/sg0
# setfacl -m g:burning:rwx /dev/sg1
# setfacl -m g:burning:rwx /dev/sg2

CMD nslcd;dbus-daemon --system;xrdp-sesman;xrdp -nodaemon