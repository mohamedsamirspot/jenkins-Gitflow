FROM ubuntu:18.04
USER root
RUN mkdir -p /var/jenkins_home /home/jenkins
# this RUN instruction creates two directories inside the Docker image: /var/jenkins_home and /home/jenkins. 
RUN groupadd jenkins \
    && useradd -g jenkins jenkins
RUN chown -R jenkins:jenkins /var/jenkins_home \
    && chown -R jenkins:jenkins /home/jenkins
WORKDIR /home/jenkins
RUN apt-get update && apt-get dist-upgrade -y && rm -rf /var/lib/apt/lists/*
RUN apt-get update && apt-get install -y \
    git \
    apt-transport-https \
    curl \
    init \
    openssh-server openssh-client \
    docker.io \
    vim \
 && rm -rf /var/lib/apt/lists/*
RUN service ssh start

#install kubectl
RUN apt update \
    && apt-get install gnupg gnupg1 gnupg2 -y
RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN touch /etc/apt/sources.list.d/kubernetes.list 
RUN echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
RUN apt-get update
RUN apt-get install -y kubectl

# Install Java
RUN apt-get update && apt-get install -y openjdk-11-jdk && rm -rf /var/lib/apt/lists/*
# same jdk of jenkins master

EXPOSE 22
ENTRYPOINT ["tail"]
CMD ["-f","/dev/null"]
# In summary, the combination of ENTRYPOINT ["tail"] and CMD ["-f","/dev/null"] sets the entry point of the Docker container to the tail command and configures it to continuously follow the output of /dev/null. This is often used as a workaround to keep the container running indefinitely without performing any specific task, effectively preventing it from exiting immediately after starting.

