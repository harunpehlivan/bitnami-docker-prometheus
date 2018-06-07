FROM bitnami/oraclelinux-extras:7-r5 as buildenv

RUN install_packages ca-certificates wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/prometheus-2.3.0-0-linux-x86_64-ol-7.tar.gz && \
    echo "decf78e566c6279cebef25cc52106ba6574be54ad5007850e12bec58caa7d101  /tmp/bitnami/pkg/cache/prometheus-2.3.0-0-linux-x86_64-ol-7.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/prometheus-2.3.0-0-linux-x86_64-ol-7.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/prometheus-2.3.0-0-linux-x86_64-ol-7.tar.gz

##################

FROM bitnami/oraclelinux-extras:7-r5
LABEL maintainer "Bitnami <containers@bitnami.com>"

COPY --from=buildenv /opt/bitnami/prometheus /opt/bitnami/prometheus
ENV BITNAMI_APP_NAME="prometheus" \
    BITNAMI_IMAGE_VERSION="2.3.0-ol-7-r0" \
    PATH="/opt/bitnami/prometheus/bin:$PATH"

RUN mkdir -p /opt/bitnami/prometheus/data/ && chmod g+rwX /opt/bitnami/prometheus/data/

EXPOSE 9090

WORKDIR /opt/bitnami/prometheus
USER 1001
ENTRYPOINT ["/opt/bitnami/prometheus/bin/prometheus"]
CMD ["--config.file=/opt/bitnami/prometheus/conf/prometheus.yml","--storage.tsdb.path=/opt/bitnami/prometheus/data","--web.console.libraries=/opt/bitnami/prometheus/conf/console_libraries","--web.console.templates=/opt/bitnami/prometheus/conf/consoles"]