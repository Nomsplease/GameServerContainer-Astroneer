FROM debian:bookworm

ENV DEBIAN_FRONTEND=noninteractive \
    DISPLAY=:0.0 \
    DISPLAY_WIDTH=1024 \
    DISPLAY_HEIGHT=768 \
    RUN_FLUXBOX=yes

#Install Layer - Common
RUN dpkg --add-architecture i386 &&\
    apt update &&\
    apt install -y software-properties-common supervisor tzdata unzip curl wget net-tools lib32gcc-s1 procps crudini &&\
    apt clean &&\
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#Install Layer - X11 and VNC
RUN apt update &&\
    apt install -y xvfb x11vnc xterm fluxbox novnc &&\
    apt clean &&\
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#Install Layer - SteamCMD
RUN echo "deb http://ftp.us.debian.org/debian bookworm main non-free" > /etc/apt/sources.list.d/non-free.list &&\
    apt-add-repository non-free &&\
    echo steam steam/question select "I AGREE" | debconf-set-selections &&\
    apt update &&\
    apt install -y steamcmd &&\
    apt clean &&\
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#Install Layer - Wine
RUN mkdir -pm755 /etc/apt/keyrings &&\
    wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key &&\
    wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/debian/dists/bookworm/winehq-bookworm.sources &&\
    apt update &&\
    apt install -y --install-recommends winehq-stable &&\
    apt clean &&\
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#Copy local files
COPY root/ /

#Exposed Ports
## Astroneer Server
EXPOSE 8777
## VNC
EXPOSE 8080

#Exposed Volumes
VOLUME /server

#Entrypoint to Setup services
ENTRYPOINT ["/entrypoint.sh"]