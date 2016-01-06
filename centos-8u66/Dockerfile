FROM centos:6.6

MAINTAINER Tian Lai <laitian912@gmail.com>

RUN yum update -y

RUN curl --insecure --junk-session-cookies --location --remote-name --silent --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u66-b17/jdk-8u66-linux-x64.rpm

RUN yum localinstall -y jdk-8u66-linux-x64.rpm
RUN rm jdk-8u66-linux-x64.rpm

RUN yum install -y  \
    less            \
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
    perl-ExtUtils-MakeMaker
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN sed -i 's|#PubkeyAuthentication yes|PubkeyAuthentication yes|g' /etc/ssh/sshd_config

ENV JAVA_HOME /usr/java/default

# Install git 2.5.2
RUN cd /usr/src;wget https://www.kernel.org/pub/software/scm/git/git-2.5.2.tar.gz;tar xzf git-2.5.2.tar.gz
RUN cd /usr/src/git-2.5.2;make prefix=/usr/local/git all;make prefix=/usr/local/git install
RUN echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc;source /etc/bashrc

# Add user jenkins to the image
RUN useradd jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

# Standard SSH port
EXPOSE 22

RUN /etc/init.d/sshd start
