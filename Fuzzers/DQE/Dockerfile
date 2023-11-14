FROM ubuntu:20.04
LABEL maintainer="OpenDBFuzz"
# common config
RUN apt-get update --yes && env DEBIAN_FRONTEND=noninteractive apt-get install openjdk-11-jdk maven git vim sudo  --yes --no-install-recommends

WORKDIR /home
RUN git clone https://github.com/tcse-iscas/dqetool.git && \
    cd dqetool && git submodule update --init

WORKDIR /home/dqetool
RUN mvn package -DskipTests