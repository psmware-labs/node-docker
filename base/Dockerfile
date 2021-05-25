#  SPDX-License-Identifier: AGPL-3.0-only

#  Copyright (C) 2021  Patrick McLean - psmware ltd

#  This program is free software: you can redistribute it and/or modify
#  it under the terms of the GNU Affero General Public License as published by
#  the Free Software Foundation, either version 3 of the License, or
#  (at your option) any later version.

#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU Affero General Public License for more details.
FROM node:lts-alpine
LABEL author="Pat McLean" \
      maintainer="Pat McLean<github@psmware.ie>"
# Avoid warnings by switching to noninteractive
## Setting up user access
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN apk --no-cache upgrade && apk add --no-cache --virtual .build-deps \
    git \
    gnupg \
    less \
    zsh \
    zip \
    curl \
    vim \
    shadow \
    openssh-client..
RUN echo "===> Moving node user and group..." \
    && groupmod -g 1001 node \
    && usermod -u 1001 node
RUN echo "===> Creating development user and group..." \
    && addgroup -g $USER_GID $USERNAME \
    && adduser -D -s /bin/zsh -G $USERNAME -u $USER_UID $USERNAME \
    # Add sudo support for non-root user
    && apk add sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && mkdir -p /app
USER ${USERNAME}
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    && touch /home/$USERNAME/.z
COPY .zshrc  /home/$USERNAME/
USER root
RUN chown -fR $USERNAME:$USERNAME /home/$USERNAME