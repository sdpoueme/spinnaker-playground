
FROM alpine:3.8
RUN apk update \
&& apk add ca-certificates \
&& apk add curl \
&& curl -LO https://storage.googleapis.com/spinnaker-artifacts/spin/$(curl -s https://storage.googleapis.com/spinnaker-artifacts/spin/latest)/linux/amd64/spin \
&& chmod +x spin
RUN update-ca-certificates
RUN mv spin /usr/local/bin/spin
RUN adduser -D -u 1000 spin
USER spin
RUN mkdir ~/.spin/
COPY config /home/spin/.spin/config
WORKDIR /home/spin
