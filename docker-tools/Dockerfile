FROM bitnami/minideb as base_node
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

RUN apt update
RUN apt install -y wget curl make git vim
RUN curl https://get.volta.sh | bash
ENV VOLTA_HOME /root/.volta
ENV PATH $VOLTA_HOME/bin:$PATH
RUN volta install node

WORKDIR /app

FROM base_node as pnpm
RUN volta install pnpm

FROM pnpm as install_package
COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json
RUN pnpm i --dir /app
