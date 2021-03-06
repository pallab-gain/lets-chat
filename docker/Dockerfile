FROM node:0.12
MAINTAINER SD Elements

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

ADD https://raw.githubusercontent.com/sdelements/lets-chat/master/package.json /usr/src/app/package.json

RUN set -x \
	&& apt-get update \
    && apt-get install -y libkrb5-dev --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* \
    && npm install --production \
	&& npm install lets-chat-ldap lets-chat-kerberos lets-chat-s3 \
	&& npm dedupe \
	&& npm cache clean \
	&& rm -rf /tmp/npm*

ADD https://github.com/sdelements/lets-chat/archive/master.tar.gz /usr/src/app/master.tar.gz

RUN tar -xzvf master.tar.gz \
    && cp -a lets-chat-master/. . \
	&& rm -rf lets-chat-master

RUN groupadd -r node \
    && useradd -r -g node node \
    && chown node:node uploads

ENV LCB_DATABASE_URI mongodb://mongo/letschat
ENV LCB_HTTP_HOST 0.0.0.0
ENV LCB_HTTP_PORT 8080
ENV LCB_XMPP_ENABLE true
ENV LCB_XMPP_PORT 5222

USER node

EXPOSE 8080
EXPOSE 5222

VOLUME ["/usr/src/app/uploads"]

CMD ["npm", "start"]
