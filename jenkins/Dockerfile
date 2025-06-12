FROM jenkins/jenkins:lts
USER root
RUN apt-get update && apt-get install -y ansible sshpass openssh-client iputils-ping
USER jenkins