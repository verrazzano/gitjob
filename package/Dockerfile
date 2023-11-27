FROM ghcr.io/oracle/oraclelinux:8-slim as build
ARG ARCH
ENV ARCH=$ARCH

COPY . /go/src/github.com/rancher/gitjob/

RUN microdnf update -y --setopt=install_weak_deps=0 --setopt=tsflags=nodocs && \
    microdnf install -y git && \
    microdnf install -y golang-1.20.10-1.module+el8.9.0+90068+08775a1f.x86_64 && \
    cd /go/src/github.com/rancher/gitjob/scripts && \
    ./build && \
    ls bin && \
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
ENTRYPOINT ["gitjob"]
