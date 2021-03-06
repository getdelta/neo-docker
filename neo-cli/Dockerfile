# neo-cli-ubuntu

FROM ubuntu:16.04
MAINTAINER Jonatan <jonatan.blockchain@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

ARG DOTNET_SDK_VERSION=2.0.0
ARG DOTNET_SDK_RELEASE=dotnet-sdk-2.0.0-linux-x64.tar.gz
ARG DOTNET_SDK_DOWNLOAD_URL=https://download.microsoft.com/download/1/B/4/1B4DE605-8378-47A5-B01B-2C79D6C55519/$DOTNET_SDK_RELEASE
# Disable dotnet usage information collection
# https://docs.microsoft.com/en-us/dotnet/core/tools/telemetry#behavior
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV PATH="/opt/dotnet:${PATH}"

ARG NEOCLI_VERSION=2.10.2
ARG NEOCLI_SHA256=6B98A51501ECA6DC38ACAEDAD4F088F3E4D448B752FFA42A4DBCC590B3EBF1BA
ARG NEOCLI_RELEASE=neo-cli-linux-x64.zip
ARG NEOCLI_DOWNLOAD_URL=https://github.com/neo-project/neo-cli/releases/download/v$NEOCLI_VERSION/$NEOCLI_RELEASE
ARG NEO_APPLOGS_DOWNLOAD_URL=https://github.com/neo-project/neo-plugins/releases/download/v$NEOCLI_VERSION/ApplicationLogs.zip
ARG NEO_IMPORTBLOCKS_DOWNLOAD_URL=https://github.com/neo-project/neo-plugins/releases/download/v$NEOCLI_VERSION/ImportBlocks.zip

RUN apt-get update && \
    apt-get -y install unzip dialog curl apt-transport-https ca-certificates && \
    apt-get -y install --no-install-recommends \
        libc6 \
        libcurl3 \
        libgcc1 \
        libgssapi-krb5-2 \
        libicu-dev \
        liblttng-ust0 \
        libssl1.0.0 \
        libstdc++6 \
        libunwind8 \
        libuuid1 \
        zlib1g \
        && \
    apt-get -y install libleveldb-dev libleveldb1v5 && \
    rm -rf /tmp/* /var/lib/apt/lists/* /var/cache/apt/*

RUN curl -sSL --output dotnet.tar.gz $DOTNET_SDK_DOWNLOAD_URL \
    && mkdir -p /opt/dotnet && tar zxf dotnet.tar.gz -C /opt/dotnet \
    && rm -rf dotnet.tar.gz

RUN curl -sSL --output neo-cli-ubuntu.zip $NEOCLI_DOWNLOAD_URL \
    && echo "$NEOCLI_SHA256 *neo-cli-ubuntu.zip" | sha256sum -c - \
    && unzip neo-cli-ubuntu.zip -d /opt/ \
    && rm -rf neo-cli-ubuntu.zip

RUN curl -sSL --output neo-applogs.zip $NEO_APPLOGS_DOWNLOAD_URL \
    && unzip neo-applogs.zip -d /opt/neo-cli/ \
    && rm -rf neo-applogs.zip

RUN curl -sSL --output neo-importblocks.zip $NEO_IMPORTBLOCKS_DOWNLOAD_URL \
    && unzip neo-importblocks.zip -d /opt/neo-cli/ \
    && rm -rf neo-importblocks.zip

COPY config.json /opt/neo-cli/config.json

EXPOSE 10333 10332
WORKDIR /opt/neo-cli
ENTRYPOINT [ "dotnet", "neo-cli.dll", "--rpc", "--log" ]
