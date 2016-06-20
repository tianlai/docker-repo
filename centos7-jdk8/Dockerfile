FROM centos:7.1.1503

RUN yum update -y
RUN yum install -y java-1.8.0-openjdk-devel.x86_64 openssh-server
RUN curl -fsSL https://get.docker.com/ | sh

# Add user jenkins to the image
RUN useradd jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

EXPOSE 22
