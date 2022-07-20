# syntax=docker/dockerfile:1
FROM ubuntu:latest AS builder
WORKDIR /home/ubuntu
RUN apt-get update && apt-get install -y curl && apt-get clean && rm -rf /var/lib/apt/lists/*
# Install Docker
RUN curl -sSL https://get.docker.com/ | sh
# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
RUN chmod +x kubectl
RUN mv kubectl /usr/local/bin/kubectl
# Install helm
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
RUN chmod +x get_helm.sh
RUN ./get_helm.sh
# Install npm
#RUN apt install nodejs -y && apt install npm -y


# Install datree
RUN apt install unzip -y
RUN curl https://get.datree.io | /bin/bash


# Install aws cli
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
RUN ./aws/install -i /usr/local/aws-cli -b /usr/local/bin



CMD ["/bin/bash"]

FROM node:alpine AS runtime
WORKDIR /home/node/
COPY --from=builder /usr/local/bin/kubectl /usr/local/bin/kubectl
COPY --from=builder /usr/local/bin/helm /usr/local/bin/helm
COPY --from=builder /usr/bin/docker /usr/bin/docker
RUN apk add gcompat
COPY --from=builder /usr/local/bin/datree /usr/local/bin/datree
RUN apk add curl
ENV ENV="/root/.ashrc"
RUN echo alias bash=/bin/sh > "$ENV"
CMD ["/bin/sh"]