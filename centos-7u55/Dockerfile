FROM centos:6.6

MAINTAINER Tian Lai <laitian912@gmail.com>

ENV TZ=America/Los_Angeles
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN yum update -y
RUN yum install -y  \
    telnet          \
    wget            \
    openssh-server  \
    openssh-client  \
    curl-devel      \
    expat-devel     \
    gettext-devel   \
    openssl-devel   \
    zlib-devel      \
    gcc             \
    perl-ExtUtils-MakeMaker \
    tar             \
    less

# Install java 7 update 55
RUN curl --insecure --junk-session-cookies --location --remote-name --silent --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u55-b13/jdk-7u55-linux-x64.rpm
RUN yum localinstall -y jdk-7u55-linux-x64.rpm
RUN rm jdk-7u55-linux-x64.rpm

# Install git 2.5.2
RUN cd /usr/src;wget https://www.kernel.org/pub/software/scm/git/git-2.5.2.tar.gz;tar xzf git-2.5.2.tar.gz
RUN cd /usr/src/git-2.5.2;make prefix=/usr/local/git all;make prefix=/usr/local/git install
RUN echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc;source /etc/bashrc

RUN wget -O /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.0.0/dumb-init_1.0.0_amd64
RUN chmod +x /usr/local/bin/dumb-init
RUN service sshd start

# Add Tini to avoid zombie issue
ENV TINI_VERSION v0.8.4
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]
