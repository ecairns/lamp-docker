FROM ubuntu:latest
MAINTAINER Ethan Cairns <ecairns@oklahoman.com>

# Install base packages
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
        software-properties-common \
        python-software-properties \
        curl 

RUN add-apt-repository -y ppa:webupd8team/java

RUN apt-get update && \
    echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && \
    apt-get install -y oracle-java8-installer

# && curl 'http://apache.claz.org/lucene/solr/6.4.2/solr-6.4.2.tgz' -o solr-6.4.2.tgz \

COPY ./solr-6.4.2.tgz /opt

RUN cd /opt && \
    tar xzf solr-6.4.2.tgz solr-6.4.2/bin/install_solr_service.sh --strip-components=2 && \
    ./install_solr_service.sh solr-6.4.2.tgz

RUN usermod -u 1000 solr
WORKDIR /var/solr/logs

EXPOSE 8983
#CMD ["cd /opt/solr && java -jar start.jar"]
