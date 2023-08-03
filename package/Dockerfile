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

COPY bin/gitjob /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

# Update env vars to make sure git 2.27 is executable
ENV PATH=/opt/rh/rh-git227/root/usr/bin:${PATH}
ENV LD_LIBRARY_PATH=/opt/rh/httpd24/root/usr/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
ENV PERL5LIB=/opt/rh/rh-git227/root/usr/share/perl5/vendor_perl:${PERL5LIB:+:${PERL5LIB}}

USER 1000
CMD ["gitjob"]
