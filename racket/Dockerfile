### Base Image
FROM andreacorallo/emacs-nativecomp

### Dependencies
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
            curl \
            git \
            gnupg \
            gpm \
            imagemagick \
            ispell \
            libacl1 \
            libasound2 \
            libcanberra-gtk3-module \
            libdbus-1-3 \
            libgif7 \
            libgnutls30 \
            libgtk-3-0 \
            libjansson4 \
            libjpeg-dev \
            liblcms2-2 \
            libm17n-0 \
            libpng16-16 \
            librsvg2-2 \
            libsm6 \
            libtiff5 \
            libx11-xcb1 \
            libxml2 \
            libxpm4 \
           openssh-client \
            texinfo \
            rlwrap \
    default-jre \
    zsh \
    wget \
    tmux \
    && rm -rf /var/lib/apt/lists/* /tmp/* /root/.cache/*

# libzmq3-dev -- may or may not need it
RUN apt-get update && apt-get install -y apt-utils python3 python3-pip  \
                 curl m4 perl libffi-dev python3-dev pkg-config  libgmp-dev  libzmq5 \
    && rm -rf /var/lib/apt/lists/* /tmp/* /root/.cache/*

### Install Racket
ENV RACKET_INSTALLER_URL="https://mirror.racket-lang.org/installers/8.1/racket-minimal-8.1-x86_64-linux-natipkg-cs.sh"
# ARG RACKET_VERSIN

RUN wget --output-document=racket-install.sh -q ${RACKET_INSTALLER_URL} && \
    echo "yes\n1\n" | sh racket-install.sh --create-dir --unix-style --dest /usr/ && \
    rm racket-install.sh

### Config Racket
RUN raco setup
RUN raco pkg config --set catalogs \
    "https://download.racket-lang.org/releases/8.1/catalog/" \
    "https://pkg-build.racket-lang.org/server/built/catalog/" \
    "https://pkgs.racket-lang.org" \
    "https://planet-compats.racket-lang.org"

### Env Var and Create Emacs User
# Fix "Couldn't register with accessibility bus" error message
ENV NO_AT_BRIDGE=1
ENV UNAME="phage" \
    GNAME="phage" \
    UHOME="/home/phage" \
    UID="1000" \
    GID="1000" \
    WORK="/mnt/work" \
    SHELL="/bin/bash"

    ### Create Emacs User and Group
COPY emacs_user.sh /tmp/

# Only for sudoers
RUN chown root /tmp/emacs_user.sh && \
    chmod 700  /tmp/emacs_user.sh && \
    /tmp/emacs_user.sh
USER phage
WORKDIR /home/phage

RUN pip3 install  --upgrade pip
RUN pip3 install  --user --no-cache-dir 'setuptools>=18.5' 'six>=1.9.0' jupyterlab && \
    mkdir -p /home/phage/.jupyter

RUN raco pkg install --auto iracket
RUN raco iracket install



