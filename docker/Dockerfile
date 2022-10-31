# Packages:
#   Package             Wanted Command
#   -------             --------------
#   bash                bash
#   coreutils           df          adds another ~1.5M
#   git                 git
#   lsblk               lsblk
#   ncurses             tput
#   sudo                sudo        adds ~1.5M
#   tmux                tmux        adds 1.5MB(?) to image size
#   util-linux-misc     lscpu

FROM alpine:latest

LABEL author="Giles Orr"
LABEL gitsource="https://github.com/gilesorr/dotbashprompt"

RUN apk add --update bash \
    && apk add coreutils \
    && apk add git \
    && apk add lsblk \
    && apk add ncurses \
    && apk add sudo \
    && apk add tmux \
    && apk add util-linux-misc \
    && rm -rf /var/cache/apk/*
RUN mkdir -p /root/really/deep/directory/structure/even/deeper/
COPY dotbashrc /root/.bashrc
COPY dotprofile /root/.profile
COPY tmux.conf /etc/tmux.conf
RUN chmod o+r /etc/tmux.conf
RUN adduser -D -h /home/penguin -g /bin/bash penguin
COPY sudoers.penguin /etc/sudoers.d/penguin

USER penguin
COPY dotbashrc /home/penguin/.bashrc
COPY dotprofile /home/penguin/.profile
RUN mkdir -p /home/penguin/really/deep/directory/structure/even/deeper/
# We want the git checkout as late as possible because we want to bust
# the cache before we run it.  Requires
# '--build-arg CACHEBUST="$(date +%s)"' or similar when building the
# image to bust the cache at this point.
ARG CACHEBUST
RUN git clone https://github.com/gilesorr/dotbashprompt.git /home/penguin/.bashprompt
USER root
#RUN chown penguin:penguin /home/penguin/.bashrc /home/penguin/.profile
# Linking a user directory with binaries for root to run is normally a
# security disaster ... but this is a demo disk and this is actually
# helpful:
RUN ln -s /home/penguin/.bashprompt /root/.bashprompt
WORKDIR /home/penguin
USER penguin
ENTRYPOINT [ "/bin/bash" ]

