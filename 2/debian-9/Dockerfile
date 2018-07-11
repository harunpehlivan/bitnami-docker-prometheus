FROM bitnami/minideb:stretch as buildenv

RUN install_packages ca-certificates wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/prometheus-2.3.1-0-linux-amd64-debian-9.tar.gz && \
    echo "b3ca2dfc95326f4815a29c47498df7665651404c9d0ce71e2f3285cb67539f7e  /tmp/bitnami/pkg/cache/prometheus-2.3.1-0-linux-amd64-debian-9.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/prometheus-2.3.1-0-linux-amd64-debian-9.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/prometheus-2.3.1-0-linux-amd64-debian-9.tar.gz

##################

FROM bitnami/minideb:stretch
LABEL maintainer "Bitnami <containers@bitnami.com>"

COPY --from=buildenv /opt/bitnami/prometheus /opt/bitnami/prometheus
ENV BITNAMI_APP_NAME="prometheus" \
    BITNAMI_IMAGE_VERSION="2.3.1-debian-9-r2" \
    PATH="/opt/bitnami/prometheus/bin:$PATH"

RUN mkdir -p /opt/bitnami/prometheus/data/ && chmod g+rwX /opt/bitnami/prometheus/data/

EXPOSE 9090

WORKDIR /opt/bitnami/prometheus
USER 1001
ENTRYPOINT ["/opt/bitnami/prometheus/bin/prometheus"]
CMD ["--config.file=/opt/bitnami/prometheus/conf/prometheus.yml","--storage.tsdb.path=/opt/bitnami/prometheus/data","--web.console.libraries=/opt/bitnami/prometheus/conf/console_libraries","--web.console.templates=/opt/bitnami/prometheus/conf/consoles"]