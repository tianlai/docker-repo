FROM ubuntu:trusty
MAINTAINER Tian Lai <laitian912@gmail.com>

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update
RUN apt-get -y upgrade

RUN apt-get install -y openssh-server
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

RUN apt-get install -y  \
    Xvfb                \
    Xorg                \
    git                 \
    curl                \
    unzip               \
    software-properties-common

RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections && \
    apt-get install -y  --force-yes oracle-java8-installer
RUN mkdir /usr/java/ && ln -snf /usr/lib/jvm/java-8-oracle /usr/java/default

RUN cd /tmp;wget http://download-origin.cdn.mozilla.net/pub/mozilla.org/firefox/releases/17.0.10esr/linux-x86_64/en-US/firefox-17.0.10esr.tar.bz2;
RUN tar xjf /tmp/firefox-17.0.10esr.tar.bz2;mv firefox/ /opt/firefox17
RUN ln -s /opt/firefox17/firefox /usr/bin/firefox

RUN adduser --quiet jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

EXPOSE 22

ENV TZ=America/Los_Angeles
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

USER jenkins
RUN curl -s http://get.sdkman.io | bash && source /home/jenkins/.sdkman/bin/sdkman-init.sh && yes|sdk install groovy 2.4.4

CMD ["/usr/sbin/sshd", "-D"]
