FROM ubuntu:20.04
LABEL maintainer="OpenDBFuzz"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update
RUN apt-get -y install wget ca-certificates sudo gnupg curl git lsb-release net-tools
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
RUN apt-get update
RUN apt-get -y install postgresql-11 postgresql-9.6 postgresql-contrib

WORKDIR /home
RUN git clone   https://github.com/sslab-gatech/apollo.git && \
   cd apollo && git submodule update --init
RUN cd apollo && install-deps.sh
RUN cd apollo && compile-libs.sh