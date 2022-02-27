FROM bitnami/minideb as base_node
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

ENV VOLTA_HOME /root/.volta
ENV PATH $VOLTA_HOME/bin:$PATH

WORKDIR /app

ARG USER_NAME
ARG GROUP_NAME
ARG PUID
ARG PGID
RUN groupadd -g ${PGID} ${GROUP_NAME}
RUN useradd -m -s /bin/bash -u ${PUID} -g ${PGID} ${USER_NAME}

RUN apt-get update \
    && apt-get install -y wget curl make git \
    && apt-get clean \
    && curl https://get.volta.sh | bash \
    && volta install node

FROM base_node as install_pnpm
RUN volta install pnpm

FROM install_pnpm as install_package
ARG USER_NAME
ADD --chown=${USER_NAME}:${USER_NAME} . /app
RUN pnpm i --dir /app
