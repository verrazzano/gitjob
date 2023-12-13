FROM ghcr.io/verrazzano/golang:v1.20.12 as build
ARG ARCH
ENV ARCH=$ARCH

COPY . /go/src/github.com/rancher/gitjob/

RUN dnf config-manager --enable ol8_appstream && \
    dnf install -y git && \
    cd /go/src/github.com/rancher/gitjob/scripts && \
    ./build && \
    ./test && \
    ./validate

FROM ghcr.io/oracle/oraclelinux:8-slim
ARG ARCH
ENV ARCH=$ARCH

RUN microdnf update -y --setopt=install_weak_deps=0 --setopt=tsflags=nodocs && \
    microdnf install -y git && \
    microdnf clean all && \
    rm -rf /var/cache/yum /var/lib/rpm/* && \
    adduser -u 1000 gituser

COPY --from=build /go/src/github.com/rancher/gitjob/bin/gitjob /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER 1000
CMD ["gitjob"]
