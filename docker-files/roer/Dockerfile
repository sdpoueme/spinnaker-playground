FROM alpine:3.8

RUN apk update \
&& apk add ca-certificates \
&& apk add curl \
&& curl -o /usr/local/bin/roer -L https://github.com/spinnaker/roer/releases/download/v0.11.3/roer-linux-amd64 \
&& chmod +x /usr/local/bin/roer \
&& apk del curl
RUN adduser -D -u 1000 roer
USER roer
WORKDIR /home/roer
