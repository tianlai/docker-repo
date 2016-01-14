FROM centos:6.6

MAINTAINER Tian Lai <laitian912@gmail.com>

RUN yum update -y

RUN curl --insecure --junk-session-cookies --location --remote-name --silent --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u66-b17/jdk-8u66-linux-x64.rpm

RUN yum localinstall -y jdk-8u66-linux-x64.rpm
RUN rm jdk-8u66-linux-x64.rpm

RUN yum install -y  \
    less            \
    bzip2           \
    which           \
    telnet          \
    wget            \
    openssh-server  \
    tar             \
    curl-devel      \
    expat-devel     \
    gettext-devel   \
    openssl-devel   \
    zlib-devel      \
    gcc             \
    gtk+            \
    libXt           \
    Xvfb            \
    unzip           \
    perl-ExtUtils-MakeMaker
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN sed -i 's|#PubkeyAuthentication yes|PubkeyAuthentication yes|g' /etc/ssh/sshd_config

ENV JAVA_HOME /usr/java/default
ENV DISPLAY :20

# Install git 2.5.2
RUN cd /usr/src;wget https://www.kernel.org/pub/software/scm/git/git-2.5.2.tar.gz;tar xzf git-2.5.2.tar.gz
RUN cd /usr/src/git-2.5.2;make prefix=/usr/local/git all;make prefix=/usr/local/git install
RUN echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc;source /etc/bashrc

# Install firefox 17
RUN cd /tmp;wget http://download-origin.cdn.mozilla.net/pub/mozilla.org/firefox/releases/17.0.10esr/linux-x86_64/en-US/firefox-17.0.10esr.tar.bz2;
RUN tar xjf /tmp/firefox-17.0.10esr.tar.bz2;mv firefox/ /opt/firefox17
RUN ln -s /opt/firefox17/firefox /usr/bin/firefox


# Add user jenkins to the image
RUN useradd jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

# Standard SSH port
EXPOSE 22

# Install sdkman & Groovy
USER jenkins
RUN curl -s http://get.sdkman.io | bash;source "/home/jenkins/.sdkman/bin/sdkman-init.sh";yes|sdk install groovy 2.4.4

USER root
RUN /etc/init.d/sshd start
