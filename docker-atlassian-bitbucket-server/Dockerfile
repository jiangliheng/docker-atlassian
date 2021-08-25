FROM openjdk:8-jdk-alpine
MAINTAINER Atlassian Bitbucket Server Team

ENV RUN_USER            daemon
ENV RUN_GROUP           daemon

# https://confluence.atlassian.com/display/BitbucketServer/Bitbucket+Server+home+directory
ENV BITBUCKET_HOME          /var/atlassian/application-data/bitbucket
ENV BITBUCKET_INSTALL_DIR   /opt/atlassian/bitbucket


# 字符集变量
ENV LANG='zh_CN.UTF-8' LANGUAGE='zh_CN:zh' LC_ALL='zh_CN.UTF-8'

# 时区变量
ENV TIME_ZONE Asia/Shanghai

VOLUME ["${BITBUCKET_HOME}"]

# Expose HTTP and SSH ports
EXPOSE 7990
EXPOSE 7999

WORKDIR $BITBUCKET_HOME

CMD ["/entrypoint.sh", "-fg"]
ENTRYPOINT ["/sbin/tini", "--"]

RUN apk add --no-cache tzdata wget curl git git-daemon openssh bash procps openssl perl ttf-dejavu tini \
    && cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime \
    && echo "${TIME_ZONE}" > /etc/timezone

COPY entrypoint.sh              /entrypoint.sh

ARG BITBUCKET_VERSION=5.11.1
ARG DOWNLOAD_URL=https://product-downloads.atlassian.com/software/stash/downloads/atlassian-bitbucket-${BITBUCKET_VERSION}.tar.gz
COPY . /tmp

RUN mkdir -p                             ${BITBUCKET_INSTALL_DIR} \
    && curl -L --silent                  ${DOWNLOAD_URL} | tar -xz --strip-components=1 -C "$BITBUCKET_INSTALL_DIR" \
    && chown -R ${RUN_USER}:${RUN_GROUP} ${BITBUCKET_INSTALL_DIR}/

COPY lib/*             /opt/bitbucket/lib/
