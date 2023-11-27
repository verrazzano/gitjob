FROM ghcr.io/oracle/oraclelinux:8-slim

RUN microdnf update -y --setopt=install_weak_deps=0 --setopt=tsflags=nodocs && \
    microdnf install -y git && \
    microdnf install -y golang-1.20.10-1.module+el8.9.0+90068+08775a1f.x86_64 && \
    cd /go/src/github.com/rancher/gitjob/scripts && \
    ./build && \
    ./test && \
    ./validate && \
    microdnf clean all && \
    rm -rf /var/cache/yum /var/lib/rpm/* && \
    adduser -u 1000 gituser

COPY bin/gitjob /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

# Update env vars to make sure git 2.27 is executable
ENV PATH=/opt/rh/rh-git227/root/usr/bin:${PATH}
ENV LD_LIBRARY_PATH=/opt/rh/httpd24/root/usr/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
ENV PERL5LIB=/opt/rh/rh-git227/root/usr/share/perl5/vendor_perl:${PERL5LIB:+:${PERL5LIB}}

USER 1000
ENTRYPOINT ["gitjob"]
