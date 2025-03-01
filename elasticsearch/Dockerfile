### This is a generated file from Dockerfile.in ###

#@follow_tag(registry.redhat.io/ubi8:latest)
FROM registry.access.redhat.com/ubi8
ENV BUILD_VERSION=6.8.1
ENV SOURCE_GIT_COMMIT=${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_COMMIT:-}
ENV SOURCE_GIT_URL=${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_URL:-}

MAINTAINER OpenShift Development <dev@lists.openshift.redhat.com>

EXPOSE 9200
EXPOSE 9300
USER 0

ARG ES_ARCHIVE_URL
ARG PROMETHEUS_EXPORTER_URL
ARG INGEST_PLUGIN_URL
ARG OPENDISTRO_URL
ARG OPENSHIFT_CI


ENV ES_PATH_CONF=/etc/elasticsearch/ \
    ES_HOME=/usr/share/elasticsearch \
    ES_VER=6.8.1.redhat-00029 \
    HOME=/opt/app-root/src \
    INSTANCE_RAM=512G \
    JAVA_VER=11 \
    JAVA_HOME=/usr/lib/jvm/jre \
    _JAVA_OPTIONS="-Dcom.redhat.fips=false" \
    NODE_QUORUM=1 \
    PROMETHEUS_EXPORTER_VER=6.8.1.2-redhat-00001 \
    INGEST_PLUGIN_VER=6.8.1.0-redhat-00003 \
    OPENDISTRO_VER=0.10.1.2-redhat-00019 \
    PLUGIN_LOGLEVEL=INFO \
    RECOVER_AFTER_NODES=1 \
    RECOVER_EXPECTED_NODES=1 \
    RECOVER_AFTER_TIME=5m \
    DHE_TMP_KEY_SIZE=2048 \
    RELEASE_STREAM=prod \
    OPENSHIFT_CI=${OPENSHIFT_CI:-false}


# COPY $REMOTE_SOURCE $REMOTE_SOURCE_DIR
# WORKDIR $REMOTE_SOURCE_DIR/app/elasticsearch

RUN packages="java-${JAVA_VER}-openjdk-headless \
              python36 \
              python3-pyyaml \
              hostname \
              openssl \
              zip \
              unzip" && \
    yum install -y ${packages} && \
    rpm -V ${packages} && \
    alternatives --set python /usr/bin/python3 && \
    yum clean all

ARG MAVEN_REPO_URL=${MAVEN_REPO_URL:-file:///artifacts/}
ENV upstream_code=${upstream_code:-"."}

COPY  ${upstream_code}/extra-jvm.options /var/tmp
COPY  ${upstream_code}/ci-env.sh /var/tmp
COPY  ${upstream_code}/install-es.sh /var/tmp
COPY artifacts /artifacts
RUN /var/tmp/install-es.sh && rm -rf /var/tmp/*

COPY  ${upstream_code}/sgconfig/ ${HOME}/sgconfig/
COPY  ${upstream_code}/index_templates/ ${ES_HOME}/index_templates/
COPY  ${upstream_code}/ingest_pipelines/ ${ES_HOME}/ingest_pipelines/
COPY  ${upstream_code}/index_patterns/ ${ES_HOME}/index_patterns/
COPY  ${upstream_code}/init/ ${ES_HOME}/init/
COPY  ${upstream_code}/probe/ ${ES_HOME}/probe/
COPY  ${upstream_code}/init.sh ${HOME}/
COPY  ${upstream_code}/run.sh ${HOME}/
COPY  ${upstream_code}/ci-env.sh ${HOME}/
COPY  ${upstream_code}/install.sh ${HOME}/
COPY  ${upstream_code}/utils/** /usr/local/bin/

RUN ${HOME}/install.sh && rm -rf /artifacts && \
  mv ${ES_HOME}/lib/elasticsearch-${ES_VER}.jar ${ES_HOME}/lib/elasticsearch-$(echo $ES_VER | cut -d'.' -f1-3).jar

WORKDIR ${HOME}
USER 1000
CMD ["sh", "/opt/app-root/src/run.sh"]

LABEL \
        License="Apache-2.0" \
        io.k8s.description="Elasticsearch container for EFK aggregated logging storage" \
        io.k8s.display-name="Elasticsearch 6" \
        io.openshift.tags="logging,elk,elasticsearch" \
        vendor="Red Hat" \
        name="openshift-logging/elasticsearch6-rhel8" \
        com.redhat.component="logging-elasticsearch6-container" \
        io.openshift.maintainer.product="OpenShift Container Platform" \
        io.openshift.build.commit.id=${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_COMMIT} \
        io.openshift.build.source-location=${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_URL} \
        io.openshift.build.commit.url=${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_URL}/commit/${CI_ORIGIN_AGGREGATED_LOGGING_UPSTREAM_COMMIT} \
        version=v6.8.1

