FROM --platform=$BUILDPLATFORM node:lts-alpine

ARG JD_SCRIPTS_URL=https://gitee.com/lxk0301/jd_scripts
ARG JD_SCRIPTS_BRANCH=master

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LANG=zh_CN.UTF-8
ENV SHELL=/bin/bash
ENV PS1="\u@\h:\w \$ "
ENV ENABLE_HANGUP=true
ENV ENABLE_WEB_PANEL=true

COPY ./ /jd

RUN apk update -f \
    && apk upgrade \
    && apk --no-cache add -f bash coreutils moreutils git wget curl nano tzdata perl openssl \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && cd /jd/panel \
    && npm install \
    && git clone -b ${JD_SCRIPTS_BRANCH} ${JD_SCRIPTS_URL} /jd/scripts \
    && cd /jd/scripts \
    && npm install \
    && npm install -g pm2 \
    && ln -sf /jd/jd.sh /usr/local/bin/jd \
    && ln -sf /jd/git_pull.sh /usr/local/bin/git_pull \
    && ln -sf /jd/rm_log.sh /usr/local/bin/rm_log \
    && ln -sf /jd/export_sharecodes.sh /usr/local/bin/export_sharecodes \
    && cp -f /jd/docker/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh \
    && chmod 777 /usr/local/bin/docker-entrypoint.sh \
    && rm -rf /root/.npm

WORKDIR /jd

ENTRYPOINT docker-entrypoint.sh