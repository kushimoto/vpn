FROM debian:latest

ENV BUILD_VER v4.29-9680

RUN apt update && apt -y upgrade

RUN apt install -y isc-dhcp-server

ADD ./dhcpd.conf /etc/dhcp/dhcpd.conf

CMD /bin/sh -c
