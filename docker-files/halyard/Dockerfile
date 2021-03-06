FROM ubuntu:16.04
RUN apt-get update -y
RUN apt-get install python python3-pip git unzip software-properties-common apt-transport-https curl procps wget ca-certificates gnupg2 -y
RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
RUN apt-get update
RUN apt-get install -y kubectl
RUN curl -o /tmp/google-cloud-sdk.tar.gz -L https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-234.0.0-linux-x86_64.tar.gz
RUN useradd hal -u 1000 --create-home --groups root -s /bin/bash
RUN tar -C /opt -zxvf /tmp/google-cloud-sdk.tar.gz \
&& chown -R hal:hal /opt/google-cloud-sdk
RUN add-apt-repository ppa:deadsnakes/ppa
RUN export GNUPGHOME="$(mktemp -d)"
RUN wget -q -O - https://dist.sapmachine.io/debian/sapmachine.old.key | gpg --batch --import
RUN gpg --batch --export --armor 'DA4C 00C1 BDB1 3763 8608 4E20 C7EB 4578 740A EEA2' > /etc/apt/trusted.gpg.d/sapmachine.old.gpg.asc
RUN wget -q -O - https://dist.sapmachine.io/debian/sapmachine.key | gpg --batch --import
RUN gpg --batch --export --armor 'CACB 9FE0 9150 307D 1D22 D829 6275 4C3B 3ABC FE23' > /etc/apt/trusted.gpg.d/sapmachine.gpg.asc
#RUN gpgconf --kill all && rm -rf "$GNUPGHOME"
RUN echo "deb http://dist.sapmachine.io/debian/amd64/ ./" > /etc/apt/sources.list.d/sapmachine.list
RUN apt-get update
RUN apt install python3.7 -y
RUN apt install build-essential -y
RUN apt-get install python3.7-dev -y
RUN apt-get install jq -y
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
RUN python3.7 get-pip.py
RUN apt-get install sapmachine-11-jdk -y --allow-unauthenticated
ENV PATH=$JAVA_HOME/bin:$PATH
USER root
RUN update-ca-certificates
RUN curl -o /tmp/install_kustomize.sh -L https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh
RUN chmod +x /tmp/install_kustomize.sh
RUN /tmp/install_kustomize.sh
RUN curl -o /tmp/InstallHalyard.sh -L https://raw.githubusercontent.com/spinnaker/halyard/master/install/debian/InstallHalyard.sh
RUN chmod +x /tmp/InstallHalyard.sh
RUN /tmp/InstallHalyard.sh --user hal --version 1.38.0 -y
COPY halyard.yml /opt/spinnaker/config/halyard.yml
RUN  pip3 install awscli --upgrade
USER hal
RUN which aws
RUN mkdir /home/hal/.aws
RUN /opt/google-cloud-sdk/install.sh
RUN /opt/google-cloud-sdk/bin/gcloud config set disable_usage_reporting true


WORKDIR /home/hal
