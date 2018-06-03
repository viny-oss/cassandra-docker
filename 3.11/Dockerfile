#########################################
# Dockerfile to run a Cassandra Container
#########################################

FROM ibmjava:8-sfj-alpine

MAINTAINER Sylvain Corsini <sylvain.corsini@pm.me> (@scorsi)

ENV      CASSANDRA_VERSION=3.11.2 \
            CASSANDRA_HOME=/opt/cassandra \
          CASSANDRA_CONFIG=/etc/cassandra \
     CASSANDRA_PERSIST_DIR=/var/lib/cassandra \
            CASSANDRA_DATA=/var/lib/cassandra/data \
       CASSANDRA_COMMITLOG=/var/lib/cassandra/commitlog \
             CASSANDRA_LOG=/var/log/cassandra \
            CASSANDRA_USER=cassandra

ENV CASSANDRA_DOWNLOAD_URL=http://artfiles.org/apache.org/cassandra/${CASSANDRA_VERSION}/apache-cassandra-${CASSANDRA_VERSION}-bin.tar.gz

RUN mkdir -p \
        ${CASSANDRA_HOME} \
        ${CASSANDRA_CONFIG} \
        ${CASSANDRA_DATA} \
        ${CASSANDRA_COMMITLOG} \
        ${CASSANDRA_LOG}

# Install dependencies
RUN apk --update add --no-cache tar wget

# Run installation
RUN wget ${CASSANDRA_DOWNLOAD_URL} -O /tmp/cassandra.tar.gz -t 5 \
    && \
    tar -xvzf /tmp/cassandra.tar.gz -C /tmp/ \
    && \
    mv /tmp/apache-cassandra-*/* ${CASSANDRA_HOME}

ENV           PATH=${PATH}:${CASSANDRA_HOME}/bin \
    CASSANDRA_CONF=${CASSANDRA_CONFIG}

# Add user and give all access to that user
RUN adduser -D -s /bin/sh ${CASSANDRA_USER} \
    && \
    chown -R ${CASSANDRA_USER}:${CASSANDRA_USER} \
        ${CASSANDRA_HOME} \
        ${CASSANDRA_PERSIST_DIR} \
        ${CASSANDRA_DATA} \
        ${CASSANDRA_CONFIG} \
        ${CASSANDRA_LOG} \
        ${CASSANDRA_COMMITLOG} \
    && \
    chmod 777 \
        ${CASSANDRA_HOME} \
        ${CASSANDRA_PERSIST_DIR} \
        ${CASSANDRA_DATA} \
        ${CASSANDRA_CONFIG} \
        ${CASSANDRA_LOG} \
        ${CASSANDRA_COMMITLOG}

# Clean image
RUN apk --purge del tar wget && rm -rf /tmp/* /var/cache/apk/* ${CASSANDRA_HOME}/*doc

USER ${CASSANDRA_USER}
WORKDIR ${CASSANDRA_HOME}
VOLUME ${CASSANDRA_PERSIST_DIR}

EXPOSE 7000 7001 7199 9042 9160

CMD ["cassandra", "-f"]
