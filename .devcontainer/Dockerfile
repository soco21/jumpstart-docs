# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.217.4/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG VARIANT="17-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/java:0-${VARIANT}

# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi


RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive \
    && DEBCONF_NONINTERACTIVE_SEEN=true \
    && sudo apt-get install -y --no-install-recommends \
        #basic tools
        git \
        ca-certificates \
        cmake \
        g++ \
        curl \
        gpp \
        lmodern \
        # libappindicator3-1 \
        libatspi2.0-0 \
        libasound2 \
        libgconf-2-4 \
        libgbm-dev \
        libgtk-3-0 \
        libnotify4 \
        libnss3 \
        libsecret-1-0 \
        libxss1 \
        libxtst6 \
        make \
        pandoc \
        sudo \
        texlive \
        texlive-extra-utils \
        texlive-latex-extra \
        texlive-pictures \
        texlive-fonts-recommended \
        texlive-fonts-extra \
        wget \
        xdg-utils \
        desktop-file-utils \
        xvfb

RUN mkdir -p /root/texmf/tex/latex

RUN wget https://github.com/langchr86/docmake/archive/refs/heads/master.zip -O /tmp/docmake-build.zip && \
    unzip /tmp/docmake-build.zip -d /tmp/docmake-unzip && \
    mv /tmp/docmake-unzip/docmake-master /tmp/docmake-build && \
    mkdir /tmp/docmake-build/install_build && \
    cmake -S /tmp/docmake-build/ -B /tmp/docmake-build/install_build && \
    cmake --build /tmp/docmake-build/install_build --target install

COPY --from=ghcr.io/langchr86/docmake:latest /usr/share/texlive/texmf-dist/tex/  /usr/share/texlive/texmf-dist/tex/ 
RUN texhash

ARG DRAWIO_VERSION=16.5.1
ARG DRAWIO_REPO=https://github.com/jgraph/drawio-desktop/releases/download/
RUN wget --no-check-certificate ${DRAWIO_REPO}v$DRAWIO_VERSION/drawio-amd64-$DRAWIO_VERSION.deb -O /tmp/drawio.deb && \
    apt install /tmp/drawio.deb

ENV DISPLAY=":99"


RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g markdownlint-cli" 2>&1

CMD [ "tail", "-f", "/dev/null" ]
