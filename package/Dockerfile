FROM ghcr.io/oracle/oraclelinux:7-slim
RUN yum -y update && \
    yum -y install openssh && \
    # software collections repo needed for git 2.x on OL7
    yum-config-manager --add-repo=http://yum.oracle.com/repo/OracleLinux/OL7/SoftwareCollections/x86_64 && \
    yum install -y rh-git227 && \
    # Set up needed to ensure git 2.27 from rh-git227 is on the path (gitjob original v0.1.30 image uses git version 2.35.3)
    ln /opt/rh/rh-git227/enable /etc/profile.d/git.sh && \
    source /etc/profile.d/git.sh && \
    git version && \
    yum -y clean all && \
    rm -rf /var/cache/yum && \
    adduser -u 1000 gituser

ENV PATH="/opt/rh/rh-git227/root/usr/bin:${PATH}"

RUN cat /etc/profile.d/git.sh >> /etc/profile && \
    cd /lib64 && \
    ln -s /opt/rh/httpd24/root/usr/lib64/libcurl-httpd24.so.4 && \
    ln -s /opt/rh/httpd24/root/usr/lib64/libnghttp2-httpd24.so.14

COPY bin/gitjob /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER 1000
CMD ["gitjob"]
