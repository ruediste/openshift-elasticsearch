FROM phusion/baseimage:latest
CMD ["/sbin/my_init"]

ENV VERSION 5.2.0

WORKDIR /
RUN apt-get update && apt-get install -y bash curl ca-certificates sudo && apt-get clean
RUN pwd && \
  ( curl -Lskj https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$VERSION.tar.gz | \
    gunzip -c - | tar xf - ) && ls && \
  mv /elasticsearch-$VERSION /elasticsearch && \
  rm -rf $(find /elasticsearch | egrep "(\.(exe|bat)$)") && \
  chmod uog+rw /elasticsearch && \
  rm -rf /tmp/* /var/tmp/*

VOLUME ["/data"]

# Copy configuration
COPY config /elasticsearch/config

# Copy run script
COPY run.sh /etc/service/elasticsearch/run
RUN chmod +x /etc/service/elasticsearch/run

# Set environment variables defaults
ENV ES_JAVA_OPTS "-Xms512m -Xmx512m"
ENV CLUSTER_NAME elasticsearch-default
ENV NODE_MASTER true
ENV NODE_DATA true
ENV NODE_INGEST true
ENV HTTP_ENABLE true
ENV NETWORK_HOST _site_
ENV HTTP_CORS_ENABLE true
ENV HTTP_CORS_ALLOW_ORIGIN *
ENV NUMBER_OF_MASTERS 1
ENV NUMBER_OF_SHARDS 1
ENV NUMBER_OF_REPLICAS 0
ENV MAX_LOCAL_STORAGE_NODES 1
ENV NAMESPACE default
ENV DISCOVERY_SERVICE elasticsearch-discovery
