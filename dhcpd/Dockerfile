FROM debian:latest as builder

ENV BUILD_VER v4.29-9680

RUN apt update && apt -y upgrade

RUN apt install -y isc-dhcp-server

ADD 

FROM debian:latest

ENV BUILD_VER v4.29-9680

COPY --from=builder /tmp/${BUILD_VER}/bin/vpnserver/hamcore.se2 /usr/vpnserver/hamcore.se2
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnserver/vpnserver /usr/vpnserver/vpnserver
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnserver/vpnserver /usr/vpnserver/vpnserver
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnbridge/hamcore.se2 /usr/vpnbridge/hamcore.se2
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnbridge/vpnbridge /usr/vpnbridge/vpnbridge
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnclient/hamcore.se2 /usr/vpnclient/hamcore.se2
COPY --from=builder /tmp/${BUILD_VER}/bin/vpnclient/vpnclient /usr/vpnclient/vpnclient
COPY --from=builder /tmp/${BUILD_VER}/bin/vpncmd/hamcore.se2 /usr/vpncmd/hamcore.se2
COPY --from=builder /tmp/${BUILD_VER}/bin/vpncmd/vpncmd /usr/vpncmd/vpncmd

RUN echo "#!/bin/sh" > /usr/bin/vpnserver \
        && echo /usr/vpnserver/vpnserver '"$@"' >> /usr/bin/vpnserver \
        && echo 'exit $?' >> /usr/bin/vpnserver \
        && chmod 755 /usr/bin/vpnserver \
        && echo "#!/bin/sh" > /usr/bin/vpnbridge \
        && echo /usr/vpnbridge/vpnbridge '"$@"' >> /usr/bin/vpnbridge \
        && echo 'exit $?' >> /usr/bin/vpnbridge \
        && chmod 755 /usr/bin/vpnbridge \
        && echo "#!/bin/sh" > /usr/bin/vpnclient \
        && echo /usr/vpnclient/vpnclient '"$@"' >> /usr/bin/vpnclient \
        && echo 'exit $?' >> /usr/bin/vpnclient \
        && chmod 755 /usr/bin/vpnclient \
        && echo "#!/bin/sh" > /usr/bin/vpncmd \
        && echo /usr/vpncmd/vpncmd '"$@"' >> /usr/bin/vpncmd \
        && echo 'exit $?' >> /usr/bin/vpncmd \
        && chmod 755 /usr/bin/vpncmd

RUN cd /usr/vpnserver \
        && ln -s /mnt/vpnserver-conf.d/vpn_server.config vpn_server.config

RUN apt update \
        && apt install -y supervisor \
        libreadline-dev \
	libncurses-dev \
        libssl-dev \
        zlib1g-dev \
        && touch /etc/supervisord.conf \
        && echo '[supervisord]'  >> /etc/supervisord.conf \
        && echo 'nodaemon=true'  >> /etc/supervisord.conf \
        && echo '[program:softether]' >> /etc/supervisord.conf \
        && echo 'command=/usr/bin/vpnserver execsvc'   >> /etc/supervisord.conf

CMD /usr/bin/supervisord -c /etc/supervisord.conf
