FROM debian:jessie

RUN apt-get update

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install ldap-utils libpam-ldapd debconf-utils

ADD ldap.conf /etc/ldap/ldap.conf
ADD nslcd.conf /etc/nslcd.conf
RUN chmod 0700 /etc/nslcd.conf
ADD nsswitch.conf /etc/nsswitch.conf

RUN echo "session optional pam_mkhomedir.so" >> /etc/pam.d/common-session