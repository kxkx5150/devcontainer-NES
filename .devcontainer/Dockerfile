FROM mcr.microsoft.com/vscode/devcontainers/dotnet:6.0-jammy

ARG USERNAME=vscode
ENV DEBIAN_FRONTEND noninteractive

ENV DISPLAY :0
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    LC_ALL=C.UTF-8 


RUN apt-get update && apt-get install -y --no-install-recommends dpkg sudo \
    dbus dbus-x11 alsa-utils pulseaudio pulseaudio-utils mesa-utils x11-apps \
    make cc65 fceux \
    mono-complete libsdl2-2.0 gnome-themes-standard libsdl2-dev libgtk2.0-dev \
    apt-transport-https \
    ca-certificates \
    cabextract \
    git \
    gnupg \
    gosu \
    gpg-agent \
    locales \
    tzdata \
    unzip \
    wget \
    winbind \
    zenity


# Install wine
ARG WINE_BRANCH="stable"
RUN wget -nv -O- https://dl.winehq.org/wine-builds/winehq.key | APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1 apt-key add - \
    && echo "deb https://dl.winehq.org/wine-builds/ubuntu/ $(grep VERSION_CODENAME= /etc/os-release | cut -d= -f2) main" >> /etc/apt/sources.list \
    && dpkg --add-architecture i386 \
    && apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --install-recommends winehq-${WINE_BRANCH} \
    && apt-get clean -y && rm -rf /var/lib/apt/lists

USER ${USERNAME}
RUN mkdir -p /home/${USERNAME}/workspace
WORKDIR /home/${USERNAME}/workspace

COPY tools/nesasm.zip /home/${USERNAME}
RUN unzip /home/${USERNAME}/nesasm.zip -d /home/${USERNAME}/nesasm/ \
    && cd /home/${USERNAME}/nesasm/nesasm-master/source && make && sudo make install


USER root
ENV PATH $PATH:/usr/games


COPY set_uid.sh /home/
ENTRYPOINT ["/home/set_uid.sh"]
COPY ./.inject_bashrc /root
RUN cat /root/.inject_bashrc >> /home/${USERNAME}/.bashrc
CMD ["/bin/sh", "-c", "echo Container started ; trap \"exit 0\" 15; while sleep 1 & wait $!; do :; done"]
